# NAME

Lexical::Importer - Importer + Lexical subs/vars.

# DESCRIPTION

This is a subclass of [Importer](https://metacpan.org/pod/Importer) which will import all symbols as lexicals
instead of package symbols.

# IMPORTANT NOTE

This imports symbols into the currently compiling scope which is not
necessarily the same as the package doing the importing.

# SYNOPSIS

Say you have a module, `Foo.pm`:

    package Foo

    use base 'Exporter';
    our @EXPORT = qw/foo/;

    sub foo { 'not lexical' }

You want to import `foo()` to use, but you also have your own `foo()` method
you do not want to squash in `Your::Module.pm`

    # Define package versions first
    sub foo { 'not lexical' }

    say foo(); # prints 'not lexical';

    {
        use Lexical::Importer Foo => 'foo';
        say foo(); # prints 'foo'
    }

    say foo(); # prints 'not lexical' again;

    use Lexical::Importer Foo => 'foo';
    say foo(); # prints 'foo'

    say __PACKAGE__->foo(); # prints 'not lexical', method dispatch find package sub.

    # Remove lexical subs
    no Lexical::Importer;
    say foo(); # prints 'not lexical' again;

# IMPORTER

This package inherits from [Importer](https://metacpan.org/pod/Importer) and works exactly the same apart from
being lexical instead of modifying the symbol table.

# SEE ALSO

[Importer](https://metacpan.org/pod/Importer) - The importer module this package subclasses

[Lexical::Var](https://metacpan.org/pod/Lexical::Var) and [Lexical::Sub](https://metacpan.org/pod/Lexical::Sub) - The awesome modules Zefram wrote that
make this possible. _Note: Lexical::Importer ships with a forked copy of these_

[Lexical::Import](https://metacpan.org/pod/Lexical::Import) - A similar module, but it does not support everything
[Lexical::Importer](https://metacpan.org/pod/Lexical::Importer) does.

# LEXICAL-VAR FORK

The [Lexical::Importer](https://metacpan.org/pod/Lexical::Importer) module is bundled with a fork of the [Lexical::Var](https://metacpan.org/pod/Lexical::Var) XS
code. This fork is necessary due to [Lexical::Var](https://metacpan.org/pod/Lexical::Var) being broken on newer
perls. The author of the original package is not accepting third party patches,
and has not yet fixed the issues himself. Once a version of [Lexical::Var](https://metacpan.org/pod/Lexical::Var)
ships with a fix for newer perls this fork will likely be removed.

## AUTHOR

Andrew Main (Zefram) <zefram@fysh.org>

## COPYRIGHT

Copyright (C) 2009, 2010, 2011, 2012, 2013
Andrew Main (Zefram) <zefram@fysh.org>

## LICENSE

This module is free software; you can redistribute it and/or modify it
under the same terms as Perl itself.

# SOURCE

The source code repository for Lexical-Importer can be found at
`http://github.com/FIXME/`.

# MAINTAINERS

- Chad Granum <exodist@cpan.org>

# AUTHORS

- Chad Granum <exodist@cpan.org>

# COPYRIGHT

Copyright 2016 Chad Granum <exodist@cpan.org>.

This program is free software; you can redistribute it and/or
modify it under the same terms as Perl itself.

See `http://dev.perl.org/licenses/`
