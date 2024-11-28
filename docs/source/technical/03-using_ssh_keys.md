**Using ssh keys, so you do not have to type your password**

# What is an ssh key

An SSH key is a secure, digital key used for authenticating and encrypting connections between computers over a network. Think of it as a special password that allows secure, remote access to another computer without needing to type a password each time.

Here’s a simple analogy: Imagine you have a locked door (your computer) and you want to allow a trusted friend (another computer) to enter without needing to give them your key each time (password). Instead, you give them a copy of a special, unique key (SSH key). Only this key can unlock the door securely.

In technical terms, an SSH key consists of two parts:
1. **Private Key**: Stays on your computer, never shared.
2. **Public Key**: Shared with the other computer you want to connect to.

When you attempt to connect to saga, the computer on the other side uses the public key to verify your identity through a secure process that involves both keys, ensuring a safe connection.

# How to create one

You should have installed putty as described in the original document. PuTTY includes the PuTTYgen program:

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/3913259b-f1f7-42ec-a36a-b72cbc5343d5)

This is what you see when it starts:

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/ac3b9045-7e2b-4bef-8efe-279ddfa519cf)

Out of the "Key" menu, click on "SSH-2 ECDSA":

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/cd46f2aa-4f41-4c10-a356-6049f238d5eb)

Then click on "Generate", then move the mouse cursor within the empty square area:

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/da63c9f3-4c78-4121-8159-0e7222cc33f6)

After 10-20 seconds you will get something like this:

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/59dc5e34-ab5d-4722-bff6-4bbd32040b92)

Please enter a password and verify it, in the following two boxes: 

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/4ff37714-f601-4e8a-8922-9c533a07cde1)

You can also use a "passphrase", meaning, instead of a word with no spaces, you can use a line out of your favorite book or two, including spaces and punctuation.

Copy the text in the "Public key for pasting into OpenSSH authorized_keys file" and save it in a file called "id_ecdsa.pub" using notepad:

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/3d44dde8-1570-4c1b-9f5f-08920269f291)


Click on "Conversions" and then on "Export OpenSSH key":

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/cd4b1d21-2588-4a78-bcc3-0d32ef7ca3c3)

Save the file as "id_ecdsa" with no extension

Click on "Save private key" and save it as "id_ecdsa". The file created will be "id_ecdsa.pkk"

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/a3252359-5214-4e7b-b399-ce6388549ee7)

and you are done! Remember the location where you saved both files for our next section


## Using the ssh keys

### Copy the public key to saga

We need to create a directory and a file in saga:

1. log into saga.
2. `mkdir ~/.ssh; chmod 700 ~/.ssh; touch ~/.ssh/authorized_keys; chmod 600 ~/.sssh/authorized_keys`
3. Open your gitbash on your computer
4. Copy over the keys

## Import the key into PuTTy

Open PuTTY, under "Saved Sessions" select "Default settings", hit load

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/a7ed3b2b-d929-4617-85e4-f44a60ed3447)

Select Connection -> SSH -> Auth -> Credentials -> Private key file for authentication -> browse

![image](https://github.com/NorwegianVeterinaryInstitute/nvi-documentation/assets/77984068/d26d241f-d92a-4325-bda5-b16adaeff837)

Open id_ecdsa.

You will be asked for the key you inserted. Type in the key.

Go back to Session on the top of the tree, on the left-hand side. click 'save' under the default session.

You are now ready to use your ss key













