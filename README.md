[![Build Status](https://travis-ci.org/stphnlyd/perl5-Eval-Quosure.svg?branch=master)](https://travis-ci.org/stphnlyd/perl5-Eval-Quosure)

# NAME

Eval::Quosure - Evaluate within a caller environment

# VERSION

version 0.001000

# SYNOPSIS

```perl
use Eval::Quosure;

sub foo {
    my $a = 2;
    my $b = 3;
    return Eval::Quosure->new('bar($a, $b, $c)');
}

sub bar {
    my ($a, $b, $c) = @_;
    return $a * $b * $c;
}

my $q = foo();

my $a = 0;  # This is not used when evaluating the quosure.
print $q->eval( { '$c' => 7 } ), "\n";
```

# DESCRIPTION

This class acts similar to R's "quosure". A "quosure" is an object
that combines an expression and an environment in which the expression
can be evaluated. 

Note that as this is string eval so is not secure. USE IT WITH CAUTION!

# CONSTRUCTION

```
new(Str $expr, $level=0)
```

`$expr` is a string. `$level` is used like the argument of `caller` and
PadWalker's `peek_my`, `0` is for the scope that creates the quosure
object, `1` is for the upper scope of the scope that creates the quosure,
and so on. 

# METHODS

## expr

Get the expression stored in the object.

## captures

Get the captured variables stored in the object. Returns a hashref with
keys being variables names including sigil and values being references
to the variables.

## caller

Get the caller info stored in the object.
Returns an arrayref of same structure as what the `caller()` returns.

## eval

```
eval(HashRef $additional_captures={})
```

Evaluate the quosure's expression in its own environment, with captured
variables from what's obtained when the quosure's created plus specified
by `$additional_captures`, which is a hashref with keys be the full name
of the variable including sigil.

# SEE ALSO

[R's "rlang" package](https://cran.r-project.org/web/packages/rlang) which
provides quosure.

[Eval::Closure](https://metacpan.org/pod/Eval::Closure), [Binding](https://metacpan.org/pod/Binding)

# AUTHOR

Stephan Loyd <sloyd@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2019 by Stephan Loyd.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
