# epoll_test
simple daemon to test LT/ET on EPOLL and blocking/nonblocking on sockfd/connfd

# make
gcc -W -Wall ET_LT_block_nonblock_test.c -o test

# run
./test 127.0.0.1 7788

# test
## To test nonblocking sockfd:
clients run:
nc 127.0.0.1 7788 &

clients kill :
$:for i in {1..5};do kill %$i;done

## To test ET/nonblocking sockfd:
Comment out L 247
Uncommet out L 250

## To test LT/blocking connfd:
Comment out L 250
Uncommet out L 247
L 263
clients run:
nc 127.0.0.1 7788
123456789

## To test LT/nonblocking connfd:
Comment out L 263
Uncomment out L 266

## To test ET/blocking connfd:
Comment out other lines
Uncomment out L 269

Comment out L 178
Uncomment out L 181

## To test ET/nonblocking connfd
Comment out other lines
Uncomment out L 272

# Summary
1. For sockfd(listen fd), better to choose LT in case of missing events in high concurrency clients-connecting scenarios. 
   If choose ET, use while loop to run accept()
2. For connfd(for read/write fd), it is almost same to use blocking/nonblocking in LT. The best way is to use nonblocking in case of some special senarios.
3. For connfd, Must use nonblocking in ET and read/write all the data in every epoll events.
