### Session 1 April 27, 2026

I finally got hermes-agent up and running with OpenAI codex using gpt5.5. Took a while and even had to subscribe to it and cancel my Claude subscription.
When I got the agent up and running I was kinda lost, didn't know what to do or what I wanted. Naturally, I just went a head and gave it a prompt to "Create me a secured and hardened ssh config file so I can reviw and deploy on the machine.
The agent did not ask any question and gave me a full ssh config file with no root login, disabled password and keyboard interactive authenticaiton, keep pubkey auth enabled, disable empty passwords, hostbased auth, X11 forwarding, agent forwarding, TCP forwarding, tunnesl, gateway ports, stream-local forwarding
Honestly, some of these rules I did know, but there was a couple I really didn't know and gave me a realization that I should maybe even use the ai to create me ssh configs for all of my machines.


