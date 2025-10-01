// supabase/functions/transcribe-audio/index.ts

import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { corsHeaders } from '../_shared/cors.ts'

// The AssemblyAI API Key will be set as an environment variable in the Supabase dashboard
const ASSEMBLYAI_API_KEY = Deno.env.get('ASSEMBLYAI_API_KEY')

// Helper function to handle polling
const poll = async (url: string, headers: HeadersInit) => {
  for (let i = 0; i < 20; i++) { // Poll for a maximum of 60 seconds
    await new Promise(resolve => setTimeout(resolve, 3000)); // Wait for 3 seconds
    
    const pollResponse = await fetch(url, { headers });
    const pollData = await pollResponse.json();

    if (pollData.status === 'completed') {
      return pollData;
    }
    if (pollData.status === 'error') {
      throw new Error(`Transcription failed: ${pollData.error}`);
    }
  }
  throw new Error('Transcription timed out. The audio might be too long.');
};

serve(async (req) => {
  // This is needed for the browser to call the function
  if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
  }

  try {
    if (!ASSEMBLYAI_API_KEY) {
      throw new Error('AssemblyAI API key is not set in the Supabase project secrets.')
    }

    const audioData = await req.blob()

    // Step 1: Upload the audio file to AssemblyAI
    const uploadResponse = await fetch('https://api.assemblyai.com/v2/upload', {
      method: 'POST',
      headers: {
        'authorization': ASSEMBLYAI_API_KEY,
        'content-type': 'application/octet-stream'
      },
      body: audioData
    })
    const uploadData = await uploadResponse.json()
    if (!uploadResponse.ok) throw new Error(`AssemblyAI upload failed: ${uploadData.error || 'Unknown error'}`)
    
    const audioUrl = uploadData.upload_url

    // Step 2: Request the transcription
    const transcriptResponse = await fetch('https://api.assemblyai.com/v2/transcript', {
      method: 'POST',
      headers: {
        'authorization': ASSEMBLYAI_API_KEY,
        'content-type': 'application/json'
      },
      body: JSON.stringify({ audio_url: audioUrl })
    })
    const transcriptData = await transcriptResponse.json()
    if (!transcriptResponse.ok) throw new Error(`AssemblyAI request failed: ${transcriptData.error || 'Unknown error'}`)
    
    const transcriptId = transcriptData.id
    const pollUrl = `https://api.assemblyai.com/v2/transcript/${transcriptId}`;
    const headers = { 'authorization': ASSEMBLYAI_API_KEY };

    // Step 3: Poll for the transcription result
    const finalTranscript = await poll(pollUrl, headers);

    return new Response(
      JSON.stringify({ transcript: finalTranscript.text }),
      { headers: { ...corsHeaders, 'Content-Type': 'application/json' } }
    )

  } catch (error) {
    console.error('Error in transcribe-audio function:', error)
    return new Response(
      JSON.stringify({ error: error.message }),
      { status: 500, headers: { ...corsHeaders, 'Content-Type': 'application/json' } }
    )
  }
})
