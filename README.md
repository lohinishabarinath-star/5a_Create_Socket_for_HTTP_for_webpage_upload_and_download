# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
##CLIENT
~~~
import socket
s = socket.socket()
s.connect(("localhost", 3024))
ch = input("1.Download  2.Upload : ")
if ch == "1":
    req = "GET / HTTP/1.1\r\nHost: localhost\r\n\r\n"
    s.send(req.encode())
    data = s.recv(4096)
    print("\n----- SERVER RESPONSE -----\n")
    print(data.decode())
else:
    msg = input("Enter data to upload: ")
    req = "POST / HTTP/1.1\r\nHost: localhost\r\n\r\n" + msg
    s.send(req.encode())
    data = s.recv(1024)
    print("\n----- SERVER RESPONSE -----\n")
    print(data.decode())
s.close()
~~~
##SERVER
~~~
import socket
s = socket.socket()
s.bind(("localhost", 3024))
s.listen(1)
print("Server running...")
while True:
    c, addr = s.accept()
    print("Connected:", addr)
    request = c.recv(4096).decode()
    print("Request received\n")
    if "GET" in request:
        f = open("index.html", "r")
        data = f.read()
        f.close()
        response = "HTTP/1.1 200 OK\r\n\r\n" + data
        c.send(response.encode())
    elif "POST" in request:
        body = request.split("\r\n\r\n")[-1]
        f = open("upload.txt", "w")
        f.write(body)
        f.close()
        response = "HTTP/1.1 200 OK\r\n\r\nFile Uploaded"
        c.send(response.encode())
    c.close()
~~~
## OUTPUT

<img width="1333" height="315" alt="image" src="https://github.com/user-attachments/assets/a1eab445-d563-40de-bf27-8c0d1d92ae19" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
