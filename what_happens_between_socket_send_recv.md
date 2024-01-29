# what happens between socket send & recv

> https://xingkunz.github.io/2020/01/09/Linux%E7%BD%91%E7%BB%9C%E5%86%85%E6%A0%B8%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90-%E5%A5%97%E6%8E%A5%E5%AD%97%E4%B9%8B%E6%94%B6%E5%8F%91%E6%95%B0%E6%8D%AE%E3%80%81%E7%9B%91%E5%90%AC%E3%80%81%E8%BF%9E%E6%8E%A5%E3%80%81%E7%BB%91%E5%AE%9A/

send()|write()|sendto() → sys_send() → sys_sendto() → sock_sendmsg() → __sock_sendmsg() → sock->prot->sendmsg() → sk->sk_prot->sendmsg()