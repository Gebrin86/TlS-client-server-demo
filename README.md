# TLS Client-Server Demo on Linux

This guide walks you step-by-step to create a simple TLS server and test it locally using OpenSSL.

---

## Step 1: Open a Terminal

- On the left taskbar, click the black square icon (Terminal).  
- Or press **Ctrl + Alt + T** on your keyboard.  

A black window should open where you can type commands.

---

## Step 2: Install OpenSSL

In the terminal, type the following commands exactly and press **Enter**:

sudo apt update  
sudo apt install -y openssl

It will ask for your password (the one you created when installing Ubuntu). When you type it, you won’t see stars or dots — that’s normal. Just type it and press Enter.  

✅ If that works, OpenSSL is installed — the main tool we’ll use to make certificates for TLS.

---

## Step 3: Make a Folder for the Project

Type the following:

mkdir ~/tls-demo  
cd ~/tls-demo

This creates a folder called `tls-demo` inside your home folder and moves you into it. You’ll see the prompt change to something like:

user@ubuntu:~/tls-demo$

---

## Step 4: Create a Server Key

Type:

openssl genrsa -out server.key 2048

- This creates a private key called `server.key`.  
- You won’t see much output, but if you type `ls`, you’ll see the file is there.

ls

You should see:

server.key

**Notes:**

- `genrsa` = generate RSA key  
- `-out server.key` = save it to a file named `server.key`  
- `2048` = key length (a common secure size)  

---

## Step 5: Create a Certificate Request

Run this command (all on one line):

openssl req -new -key server.key -out server.csr -subj "/C=US/ST=TX/L=Dallas/O=TestLab/OU=Dev/CN=localhost"

- Uses your `server.key`  
- Creates a request file called `server.csr`  

Check that both files exist:

ls

You should see:

server.key  server.csr

---

## Step 6: Create a Self-Signed Server Certificate

Run this command:

openssl x509 -req -in server.csr -signkey server.key -out server.crt -days 365

- Uses the request (`server.csr`)  
- Signs it with your key (`server.key`)  
- Produces a certificate called `server.crt`, valid for 1 year  

Check files again:

ls

You should see:

server.key  server.csr  server.crt

✅ Now you have the three essential TLS files:

- `server.key` → your private key  
- `server.csr` → the request (can be ignored now)  
- `server.crt` → your self-signed certificate  

---

## Step 7: Start a Simple TLS Server

Run this command:

openssl s_server -accept 4443 -cert server.crt -key server.key

- `s_server` = OpenSSL’s built-in test TLS server  
- `-accept 4443` = listens on port 4443  
- `-cert server.crt` = uses the certificate  
- `-key server.key` = uses the private key  

Your terminal should say something like:

ACCEPT

It will now wait for connections.

---

## Step 8: Test the TLS Connection (in a New Terminal)

Open a second terminal window and run:

openssl s_client -connect localhost:4443

If everything works, you’ll see TLS handshake info and a prompt where you can type.  

For example, typing:

Hello

Will send the text to the server.  

---

✅ You now have a fully functional local TLS client-server setup using OpenSSL.
