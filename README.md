# NAME

Task::MemManager::CMalloc - Allocates buffers using C's malloc

# VERSION

version 0.04

# SYNOPSIS

    use Task::MemManager::CMalloc;

    my $buffer = Task::MemManager::CMalloc::malloc(10, 1, 'A');
    my $buffer_address = Task::MemManager::CMalloc::get_buffer_address($buffer);
    Task::MemManager::CMalloc::free($buffer);

# DESCRIPTION

The `Task::MemManager::CMalloc` module provides access to memory bufffers
allocated using C's malloc function. The buffers are allocated immediately,
i.e., not using the delayed allocation mechanism one would expect from a
garden variety (e.g. glibc) malloc implementation. The module provides
methods to allocate uninitialized, zero initialized or custom initialized
buffers, access to the buffer's memory address and facilities to free the
buffer. The module is intended to be used in conjunction with the
`Task::MemManager` module, and thus it is probably best not to use these
functions directly. 

# METHODS

- `malloc($num_of_items, $size_of_each_item, $init_value)`

    Allocates a buffer of size `$num_of_items * $size_of_each_item` bytes. If
    `$init_value` is not defined, the buffer is not initialized. If `$init_value`
    is the string 'zero', the buffer is zero initialized. Otherwise, the buffer is
    initialized with the value of `$init_value` repeated for the entire buffer.
    The value returned is processed by the `Task::MemManager` module in order to
    grab the memory address of the buffer just generated.

- `free($buffer)`

    Frees the buffer allocated by `malloc`.

- `get_buffer_address($buffer)`

    Returns the memory address of the buffer as a Perl scalar.

- `consume($external_buffer_ref, $length)`

    Consumes an external buffer, whose address is stored in a scalar variable, with 
    the latter passed as a reference to simulate pass-by-reference in C). The length 
    of the buffer should be provide. The value of the reference to the address of 
    the external buffer is then zeroed out to prevent double free from a subsequent 
    call to C's free. Internally C's realloc is used to grow/shrink the buffer to
    the desired length, and thus an implicit copy may be made. If realloc returns
    NULL, then a warning is issued that the user should not assume that the requested
    buffer length is correct.
    Note that the `$external_buffer` should be a valid Perl integer, otherwise the
    consumption will fail.

# DIAGNOSTICS

There are no diagnostics that one can use. The module will die if the
allocation fails, so you don't have to worry about error handling. 
If you set up the environment variable DEBUG to a non-zero value, then
a number of sanity checks will be performed, and the module will die
with an (informative message ?) if something is wrong.

# DEPENDENCIES

The module depends on the `Inline::C` module to compile the C code for 
the memory allocation and deallocation functions.

# TODO

None I can think of, but open to suggestions. 

# SEE ALSO

- [https://metacpan.org/pod/Inline::C](https://metacpan.org/pod/Inline::C)

    Inline::C is a module that allows you to write Perl subroutines in C. 

# AUTHOR

Christos Argyropoulos, `<chrisarg at cpan.org>`

# COPYRIGHT AND LICENSE

This software is copyright (c) 2024 by Christos Argyropoulos.

This is free software; you can redistribute it and/or modify it under the
MIT license. The full text of the license can be found in the LICENSE file
See [https://en.wikipedia.org/wiki/MIT\_License](https://en.wikipedia.org/wiki/MIT_License) for more information.
