# bugprone-bad-signal-to-kill-thread

Finds `pthread_kill` function calls when a thread is terminated by
raising `SIGTERM` signal and the signal kills the entire process, not
just the individual thread. Use any signal except `SIGTERM`.

```c++
pthread_kill(thread, SIGTERM);
```

This check corresponds to the CERT C Coding Standard rule [POS44-C. Do
not use signals to terminate
threads](https://wiki.sei.cmu.edu/confluence/display/c/POS44-C.+Do+not+use+signals+to+terminate+threads).

[cert-pos44-c]{.title-ref} redirects here as an alias of this check.
