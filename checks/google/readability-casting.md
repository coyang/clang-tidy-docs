# google-readability-casting

Finds usages of C-style casts.

<https://google.github.io/styleguide/cppguide.html#Casting>

Corresponding cpplint.py check name: [readability/casting]{.title-ref}.

This check is similar to `-Wold-style-cast`, but it suggests automated
fixes in some cases. The reported locations should not be different from
the ones generated by `-Wold-style-cast`.
