# GPT-Live Real-Time Voice Conversations

GPT-Live supports listening and speaking simultaneously, bidirectional subtitles, and background tasks. ChatGPT voice calls in Studio use `gpt-live-1` to process speech, while complex questions continue to be handled by the currently selected chat model.

## Connection

Use a Platform API Token to connect to `wss://realtime.acedata.cloud/v1/live/sessions`. Use `wss://realtime.acedata.cloud/aichat2/live` for Studio or chat service credentials. The server authenticates through `Authorization: Bearer <token>`; browsers pass credentials through the WebSocket subprotocol to avoid writing them in the URL:

```javascript
const socket = new WebSocket('wss://realtime.acedata.cloud/aichat2/live', [
  'live', `acedata-token.${token}`
]);
socket.onopen = () => socket.send(JSON.stringify({
  type: 'session.start',
  session: {
    model: 'gpt-live-1',
    instructions: 'Answer concisely and friendly, using the user’s language. Hand complex questions to the background.',
    audio: { format: { type: 'audio/pcm', rate: 24000 }, output: { voice: 'marin' } },
    delegation: { type: 'client' },
    store: false
  }
}));
```

After waiting for `session.started`, continuously send `session.input_audio.append`, where `audio` is the Base64 of raw 24 kHz, mono, 16-bit little-endian PCM audio, without a WAV file header. Send audio at the actual sampling rate, and keep silent segments continuous as well.

Play `session.output_audio.delta.delta` in order. User and assistant subtitles come from `session.input_transcript.delta` and `session.output_transcript.delta`, respectively; retain the `delta`, `start_ms`, and `end_ms` of each segment; subtitles from both sides may grow simultaneously. Live does not have an end event for each individual voice response; the “responding” state should be updated based on the local playback queue.

## Background Tasks

The `client` delegation mode is currently supported. After receiving `session.delegation.created`, the application calls its own backend based on voice subtitles and saved context, and returns the result through `session.thinking.append` or `session.commentary.append`, carrying the corresponding `delegation_id` and plain-text `content` of up to 500 tokens. Results should come from completed backend operations; interrupted speech does not mean that a background task has been canceled.

Managed Responses delegation, recording storage, `session.update`, session recovery, or automatic reconnection are not supported for now. The model is fixed to `gpt-live-1`, audio is fixed to 24 kHz PCM16, and the voice is selected when the call starts. The old Realtime API can still be used.

## Billing and Ending Calls

Voice is billed by active session duration: **0.01 Credits/second, or 0.6 Credits/minute**, without rounding up to full minutes. Time while the user is speaking, the assistant is speaking, during silence, while both sides are silent, and while waiting for background tasks is all billed. Background model or tool calls are billed separately according to the corresponding API. The USD price corresponding to Credits depends on the top-up package and is subject to the current price in the console.

`session.usage.updated.usage.seconds` is cumulative usage. For example, if 12 seconds are received first and then 15 seconds are received, the total is 15 seconds. The platform only settles newly added duration and does not repeatedly accumulate snapshots.

When ending, send `session.close`, continue receiving until `session.closed` is received, then close the connection and release the microphone. A forced disconnection may cause final usage to remain unconfirmed, and the interface should clearly report this. The connection will end when the balance is insufficient, billing fails, or the session limit is reached. A single call defaults to a maximum of 30 minutes.

Parameter errors are returned through the `error` event; `error.client_event_id` corresponds to the failed command. If startup fails, recording should stop and the error should be displayed, without automatically switching models or retrying in a way that generates new charges.