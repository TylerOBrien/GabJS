# GabJS
Framework built for communicating over the WebSocket protocol using JSON-based messages. GabJS uses event-based messages to tell you that a JSON command has been received and is valid (or, depending on the circumstance, invalid.) 

# Example
```cpp
#include "GabJS/Server.hpp"

#include <iostream>

void do_repeat(gabjs::SocketP socket, gabjs::JSONP data) {
  std::cout << socket << " sent:" << std::endl;
  
  int ntimes = data->geti("ntimes");
  
  while (ntimes--) {
    std::cout << data->gets("message") << std::endl;
  }
}

int main() {
  gabjs::Server server("127.0.0.1", 5000);
  
  uint64_t repeat_id = server.add_command("repeat", {"message:string", "ntimes:int"});
  
  for (;;) {
    server.update();
    while (gabjs::Event event = server.poll()) {
      if (repeat_id == event.type) {
        do_repeat(event.socket, event.data);
      }
    }
  }
}
```
