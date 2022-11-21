# rdma_library

many-to-many rdma library

## Installation

```sh
$ mkdir build
$ cd build
$ cmake ..
$ make
```

## Running

```sh
$ ./myRDMA <each node ip>
```

## Code

### 수정해야할 것

```c
#include "myRDMA.hpp"
#define port xxxxx                                        // 사용할 포트
#define num_of_node 4                                     // 사용할 노드 개수      
#define server_ip "xxx.xxx.x.xx1"                         // 일대다 통신, 다대일 통신 때 일에 해당하는 노드 IP

string node[num_of_node] = {server_ip,"xxx.xxx.x.xx2",    // 사용할 노드들의 IP
                          "xxx.xxx.x.xx3","xxx.xxx.x.xx4"};   
```

### 통신 전 단계
```c
myRDMA myrdma;

myrdma.initialize_rdma_connection(argv[1], node, num_of_node, port, send_buffer, recv_buffer);
myrdma.create_rdma_info();
myrdma.send_info_change_qp();
```

### RDMA 통신
```c
/* many-to-many communication */
string opcode = "send" //send, send_with_imm, write, write_with_imm
string msg = "Hi many-to-many communication!"
myrdma.rdma_comm(opcode, msg);

/* 1-to-many communication */
if(is_server(ip)){
  msg = "Hi one-to-many communication!";
  myrdma.rdma_one_to_many_send_msg(opcode, msg);
}
else{
  myrdma.rdma_one_to_many_recv_msg(opcode);
}

/* many-to-1 communication */
if(is_server(ip)){
  myrdma.rdma_many_to_one_recv_msg(opcode);
}
else{
  msg = "Hi many-to-one communication!";
  myrdma.rdma_many_to_one_send_msg(opcode,msg);
}
```

### RDMA 통신 종료
```c
myrdma.exit_rdma();
```
