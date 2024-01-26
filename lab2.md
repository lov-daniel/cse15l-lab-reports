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

![](/lab2_images/)

