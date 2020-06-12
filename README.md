# select-poll-epoll
## select的使用（最多可监听1024个文件描述符）
### int select(int nfds,fd_set * readfds,fd_set * writefds,fd_set * exceptfds,struct timeval * timeout);
#### nfds:监控的文件描述符集里最大文件描述符加1
#### readfds:监控有读数据到达文件描述符集合
#### writefds:监控写数据到达文件描述符集合
#### exceptfds:监控异常发生达文件描述符集合
#### timeout:NULL,永远等待下去
####        :设置timeval,等待固定时间
####        :设置timeout里面均为0，表示立刻返回
#### struct timeval{
####      long tv_sec;//秒
####      long tv_usec;//毫秒
#### };
### void FD_CLR(int fd,fd_set * set);//把文件描述符集合里fd清0；
### void FD_ISSET(int fd,fd_set * set);//测试文件描述符集合里fd是否置1
### void FD_SET(int fd,fd_set * set);//将文件描述符fd加入set；
### void FD_ZERO(int fd,fd_set * set);//将fd从set中清除

## poll的使用（可以修改文件监听数目）
### int poll(struct pollfd * fds,int nfds,int timeout);
#### struct pollfd{
####      int fd; //文件描述符
####      short events; //监控的事件
####      short revents; //监控中满足条件的返回事件
#### };
#### events  一般为POLLIN,POLLOUT,POLLERR
#### timeout:-1表示阻塞，0表示立刻返回；>0表示等待的时间，单位是毫秒
#### nfds:监控数组中有多少文件描述符需要被监控

## epoll的使用（可以修改文件监听数目）
### int epoll_create(int size);//size表示监听的数目（建议的），创建句柄，返回epfd

### int epoll_ctl(int epfd,int op,struct epoll_event * event);//注册、修改、删除监听事件
#### epfd为epoll_create返回的句柄
#### op：EPOLL_CTR_ADD(注册新的fd到epfd);EPOLL_CTR_MOD（修改已经注册的fd的监听事件）,EPOLL_CTR_DEL(从epfd删除一个fd)
#### event:告诉内核需要监听的事件
#### struct epoll_event{
####   __uint32_t events;//事件(EPOLLIN、EPOLLOUT、EPOLLERR)
####   epoll_data_t data;//数据
#### }
#### typedef union epoll_data{
#### void * ptr;
#### int fd;
#### uint32_t u32;
#### uint64_t u64;
#### }epoll_data_t;

### int epoll_wait(int epfd,struct epoll_event * events,int maxevents,int timeout);
#### epfd为句柄
#### events为数组，返回的事件的集合
#### maxevents表示events的大小，小于size
#### timeout超时的时间（0：立刻返回，-1表示阻塞，>0表示毫秒）


