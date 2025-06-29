# cppcoreguidelines-interfaces-global-init

This check flags initializers of globals that access extern objects, and
therefore can lead to order-of-initialization problems.

This check implements
[I.22](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Ri-global-init)
from the C++ Core Guidelines.

Note that currently this does not flag calls to non-constexpr functions,
and therefore globals could still be accessed from functions themselves.
