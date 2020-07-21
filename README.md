# CSML Livechat demo

Example CSML chatbot with Livechat support

## Getting started

To get started, add a Livechat platform integration in CSML Studio, then create a flow such as this one:

```cpp
start:
  // The user has to specifically request a livechat session.
  // A simple solution for this is to create a dedicated flow
  // that handles all the livechat request lifecycle

  // Start by providing a button to initiate the livechat session.
  // This button must have a payload of LIVECHAT_SESSION_START
  say Question(
    "Do you need some help from a human agent?",
    buttons=[Button("Yes, talk to a human!", payload="LIVECHAT_SESSION_START")]
  )
  hold

  // If the user clicks on the button, a livechat session
  // with your Livechat platform will be started
  // and the bot will not receive any more messages until the session ends

  // The first event received by the bot after a session has been terminated
  // is LIVECHAT_SESSION_END
  if (event == "LIVECHAT_SESSION_END") {
    say "Thanks for chatting with us today!"
    goto end
  }

  // If the session expired (no message exchanged in the past 30 minutes),
  // the bot will receive this event when the user says something next
  if (event == "LIVECHAT_SESSION_EXPIRED") {
    say "Your livechat session expired."
    goto end
  }

  // Let's make sure that the user clicked on the button.
  // If they did click on the button, the bot will have stopped anyway!
  // This is required to make sure that livechat sessions are not started by mistake:
  // the user absolutely must confirm they did click on the button.
  say "Please click on the button to confirm you need human assistance."
  goto start
```

That's all there is to it! Read the [CSML Studio documentation](https://docs.csml.dev/studio/getting-started/livechat) for more information on how to setup a Livechat platform.
