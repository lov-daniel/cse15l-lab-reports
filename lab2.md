# Lab 2 (Servers and SSH Keys)

## Chat Server Implementation

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {

    // New messages will be appended to this string
    String messages = "";

    public String handleRequest(URI url) {

        // If no message is being added, simply display the messages.
        if (url.getPath().equals("/")) {
            return messages;
        } else {

            // If a message is getting added, append the formatted string to the messages variable.
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                    if (parameters[0].equals("s")) {
                        
                        String[] message_info = parameters[1].split("&");

                        if (message_info[1].equals("user")) {

                            messages += String.format("%s: %s\n", parameters[2], message_info[0]);
                            return messages;
                        }
                } 
            }
        }
        
        // Invalid path result
        return "404 Not Found!";
     }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

## Usage Screenshots

![Image](/lab2_images/first_image.png)

- In the screenshot above, the ```handleRequest``` method is being called with the argument of ```new URI("https://localhost:4000/add-message?s=hello&user=sophia")``` to be passed in.
- Since the URI contains ```/add-message```, the method calls ```getQuery()``` and then calls ```.split("=")``` to separate each part of the query, resulting in the String array named ```parameters``` containing ```[s, hello&user, sophia]```.
- The method then checks if ```s``` was the first part of the query, and since it is, the information about the message is then obtained by calling ```.split("&")``` on ```parameters[1]```. The result is a second String array named ```message_info```, which stores ```[hello, user]```.
- The method checks if ```user``` is stored inside of the ```message_info``` String array, and since it is, it concatenates a formatted string to the ```messages``` variable, which is then returned, displaying the message ```sophia: hello```.
- The String ```messages``` is used to store all the messages. Each message is simply concatenated to ```messages``` upon each successful call.
- The formatted string uses ```parameters[2]```, which is the name of the user (obtained in step 2) and ```message_info[0]```, which contains the actual message (obtained in step 3).
- ```messages``` at the end of this screenshot is ```"sophia: hello\n"```.

![Image](/lab2_images/second_image.png)

- In the screenshot above, the ```handleRequest``` method is being called with the argument of ```new URI("https://localhost:4000/add-message?s=halloooooo&user=daniel")``` to be passed in.
- Since the URI contains ```/add-message```, the method calls ```getQuery()``` and then calls ```.split("=")``` to separate each part of the query, resulting in the String array named ```parameters``` containing ```[s, halloooooo&user, daniel]```.
- The method then checks if ```s``` was the first part of the query, and since it is, the information about the message is then obtained by calling ```.split("&")``` on ```parameters[1]```. The result is a second String array named ```message_info```, which stores ```[halloooooo, user]```.
- The method checks if ```user``` is stored inside of the ```message_info``` String array, and since it is, it concatenates a formatted string to the ```messages``` variable, which is then returned, displaying the message ```daniel: halloooooo```.
- The String ```messages``` is used to store all the messages. Each message is simply concatenated to ```messages``` upon each successful call.
- The formatted string uses ```parameters[2]```, which is the name of the user (obtained in step 2) and ```message_info[0]```, which contains the actual message (obtained in step 3).
- ```messages``` at the end of this screenshot is ```"sophia: hello\ndaniel: halloooooo\n"```.

## SSH Keys

![Image](/lab2_images/ssh_location.png)
- The absolute path for my ```private key``` is ```C:\Users\dlov\.ssh\id_rsa```, which is located on my personal computer.

![Image](/lab2_images/ssh_location_public.png)
- The absolute path for my ```public key``` is ```/home/linux/ieng6/oce/90/dlov/.ssh/authorized_keys```, which is located on the ```ieng6``` computer.

After the running the ```scp``` command and copying the public key onto my remote account, I was able to log into my ```ieng6``` remote account without being prompted to give a passphrase.

![Image](/lab2_images/ssh_image.png)

## Reflection

Prior to the lab during week three, I had no idea how ```ssh``` worked or what it was. I am beginning to understand its uses both within the scope of this course and some possible uses of it in industry after talking to some of the tutors. The ability to access a remote computer from a terminal is something that I did not know what possible.
