# modernize-shrink-to-fit

Replace copy and swap tricks on shrinkable containers with the  
shrink_to_fit() method call.

将可收缩容器上的复制和交换技巧替换为 shrink_to_fit() 方法调用。

The shrink_to_fit() method is more readable and more effective than  
the copy and swap trick to reduce the capacity of a shrinkable  
container. Note that, the shrink_to_fit() method is only available in  
C++11 and up.

shrink_to_fit() 方法比复制和交换技巧更易读且更高效，用于减少可收缩容器的容量。  
请注意，shrink_to_fit() 方法仅在 C++11 及更高版本中可用。
