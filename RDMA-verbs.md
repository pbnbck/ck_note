### Verbs API

Verbs API是一组用于使用RDMA服务的最基本的软件接口，也就是说业界的RDMA应用，要么直接基于这组API编写，要么基于在Verbs API上又封装了一层接口的各种中间件编写。

Verbs API向用户提供了有关RDMA的一切功能，典型的包括：注册MR、创建QP、Post Send、Poll CQ等等。

对于Linux系统来说，Verbs的功能由rdma-core和内核中的RDMA子系统提供，分为用户态Verbs接口和内核态Verbs接口，分别用于用户态和内核态的RDMA应用。

### IB_VERBS

接口以ibv_xx（用户态）或者ib_xx（内核态）作为前缀，是最基础的编程接口，使用IB_VERBS就足够编写RDMA应用了。

比如：

- ibv_create_qp() 用于创建QP
- ibv_post_send() 用于下发Send WR
- ibv_poll_cq() 用于从CQ中轮询CQE

### RDMA_CM

以rdma_为前缀，主要分为两个功能：

#### CMA（Connection Management Abstraction）

在Verbs API基础上实现的，用于CM建链并交换信息的一组接口。CM建链借鉴了Socket的流程和API，底层是基于QP实现的，从用户的角度来看，是在通过QP交换之后数据交换所需要的QPN，Key等信息。

比如：

- rdma_listen()用于监听链路上的CM建链请求。
- rdma_connect()用于确认CM连接。

### CM VERBS

RDMA_CM也可以用于数据交换，相当于在verbs API上又封装了一套数据交换接口。

比如：

- rdma_post_read()可以直接下发RDMA READ操作的WR，而不像ibv_post_send()，需要在参数中指定操作类型为READ。
- rdma_post_ud_send()可以直接传入远端QPN，指向远端的AH，本地缓冲区指针等信息触发一次UD SEND操作。

上述接口虽然方便，但是需要配合CMA管理的链路使用，不能配合Verbs API使用。

Verbs API除了IB_VERBS和RDMA_CM之外，还有MAD（Management Datagram）接口等。