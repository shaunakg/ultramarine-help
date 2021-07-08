# How Ultramarine works

## Typical usage
Here's a rundown of how you would expect to use Ultramarine.

1. Create a chat by navigating to /chat/\<your chat name\>.
2. Enter a **password** and a username, and connect to the chat.
3. Send a message.
4. Recieve a message
5. *Share the chat link to allow others to view your messages* [not covered below]

## What happens in the background?
The step numbers here correlate with the steps above in *Typical usage*. Note that the **client** is you and your browser, and the **server** is the Ultramarine server. 

1. Your browser checks in with the Ultramarine server to say that it wants to register itself as a participant in the chat.
    - The server allocates your client to its own chatroom, in which messages can be transferred to you.
2. Your client stores the password and username in its memory. **At no point is the password transferred to the server**.
    - This is the key tenet of **end-to-end (E2E) encryption** - even if our servers were compromised, at no point would your message content be leaked in an unencrypted format. Ultramarine cannot read your messages.
3. When you hit 'send', your client uses [Blowfish encryption](https://en.wikipedia.org/wiki/Blowfish_(cipher)) to encrypt your messages with the password and then transfers the encrypted content to the server.
    - The server then pushes this message to all other clients in the same chatroom.
4. When you recieve a messsage, your client attempts to decrypt it with the password that you have supplied.
    - The message includes both a flag and the content of the message itself. The flag is known to both clients, and if the decrypted flag doesn't match up with what the client already knows, then the decryption password is wrong. In this case, you will see a `<unable to decrypt>` message.
    - If the flags match, then the content is decrypted and shown to you. We also use [Statically](https://statically.io/) to render a profile picture based off your username.
