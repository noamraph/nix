## [`auto-allocate-uids`]{#xp-feature-auto-allocate-uids}

Allows Nix to automatically pick UIDs for builds, rather than creating
`nixbld*` user accounts. See the [`auto-allocate-uids`](@docroot@/command-ref/conf-file.md#conf-auto-allocate-uids) setting for details.

Refer to [auto-allocate-uids tracking issue](https://github.com/NixOS/nix/milestone/34) for feature tracking.

## [`ca-derivations`]{#xp-feature-ca-derivations}

Allow derivations to be content-addressed in order to prevent
rebuilds when changes to the derivation do not result in changes to
the derivation's output. See
[__contentAddressed](@docroot@/language/advanced-attributes.md#adv-attr-__contentAddressed)
for details.

Refer to [ca-derivations tracking issue](https://github.com/NixOS/nix/milestone/35) for feature tracking.

## [`cgroups`]{#xp-feature-cgroups}

Allows Nix to execute builds inside cgroups. See
the [`use-cgroups`](@docroot@/command-ref/conf-file.md#conf-use-cgroups) setting for details.

Refer to [cgroups tracking issue](https://github.com/NixOS/nix/milestone/36) for feature tracking.

## [`configurable-impure-env`]{#xp-feature-configurable-impure-env}

Allow the use of the [impure-env](@docroot@/command-ref/conf-file.md#conf-impure-env) setting.

Refer to [configurable-impure-env tracking issue](https://github.com/NixOS/nix/milestone/37) for feature tracking.

## [`daemon-trust-override`]{#xp-feature-daemon-trust-override}

Allow forcing trusting or not trusting clients with
`nix-daemon`. This is useful for testing, but possibly also
useful for various experiments with `nix-daemon --stdio`
networking.

Refer to [daemon-trust-override tracking issue](https://github.com/NixOS/nix/milestone/38) for feature tracking.

## [`dynamic-derivations`]{#xp-feature-dynamic-derivations}

Allow the use of a few things related to dynamic derivations:

  - "text hashing" derivation outputs, so we can build .drv
    files.

  - dependencies in derivations on the outputs of
    derivations that are themselves derivations outputs.

Refer to [dynamic-derivations tracking issue](https://github.com/NixOS/nix/milestone/39) for feature tracking.

## [`fetch-closure`]{#xp-feature-fetch-closure}

Enable the use of the [`fetchClosure`](@docroot@/language/builtins.md#builtins-fetchClosure) built-in function in the Nix language.

Refer to [fetch-closure tracking issue](https://github.com/NixOS/nix/milestone/40) for feature tracking.

## [`fetch-tree`]{#xp-feature-fetch-tree}

Enable the use of the [`fetchTree`](@docroot@/language/builtins.md#builtins-fetchTree) built-in function in the Nix language.

`fetchTree` exposes a generic interface for fetching remote file system trees from different types of remote sources.
The [`flakes`](#xp-feature-flakes) feature flag always enables `fetch-tree`.
This built-in was previously guarded by the `flakes` experimental feature because of that overlap.

Enabling just this feature serves as a "release candidate", allowing users to try it out in isolation.

Refer to [fetch-tree tracking issue](https://github.com/NixOS/nix/milestone/31) for feature tracking.

## [`flakes`]{#xp-feature-flakes}

Enable flakes. See the manual entry for [`nix
flake`](@docroot@/command-ref/new-cli/nix3-flake.md) for details.

Refer to [flakes tracking issue](https://github.com/NixOS/nix/milestone/27) for feature tracking.

## [`git-hashing`]{#xp-feature-git-hashing}

Allow creating (content-addressed) store objects which are hashed via Git's hashing algorithm.
These store objects will not be understandable by older versions of Nix.

Refer to [git-hashing tracking issue](https://github.com/NixOS/nix/milestone/41) for feature tracking.

## [`impure-derivations`]{#xp-feature-impure-derivations}

Allow derivations to produce non-fixed outputs by setting the
`__impure` derivation attribute to `true`. An impure derivation can
have differing outputs each time it is built.

Example:

```
derivation {
  name = "impure";
  builder = /bin/sh;
  __impure = true; # mark this derivation as impure
  args = [ "-c" "read -n 10 random < /dev/random; echo $random > $out" ];
  system = builtins.currentSystem;
}
```

Each time this derivation is built, it can produce a different
output (as the builder outputs random bytes to `$out`).  Impure
derivations also have access to the network, and only fixed-output
or other impure derivations can rely on impure derivations. Finally,
an impure derivation cannot also be
[content-addressed](#xp-feature-ca-derivations).

This is a more explicit alternative to using [`builtins.currentTime`](@docroot@/language/builtins.md#builtins-currentTime).

Refer to [impure-derivations tracking issue](https://github.com/NixOS/nix/milestone/42) for feature tracking.

## [`local-overlay-store`]{#xp-feature-local-overlay-store}

Allow the use of [local overlay store](@docroot@/command-ref/new-cli/nix3-help-stores.md#local-overlay-store).

Refer to [local-overlay-store tracking issue](https://github.com/NixOS/nix/milestone/50) for feature tracking.

## [`mounted-ssh-store`]{#xp-feature-mounted-ssh-store}

Allow the use of the [`mounted SSH store`](@docroot@/command-ref/new-cli/nix3-help-stores.html#experimental-ssh-store-with-filesystem-mounted).

Refer to [mounted-ssh-store tracking issue](https://github.com/NixOS/nix/milestone/43) for feature tracking.

## [`nix-command`]{#xp-feature-nix-command}

Enable the new `nix` subcommands. See the manual on
[`nix`](@docroot@/command-ref/new-cli/nix.md) for details.

Refer to [nix-command tracking issue](https://github.com/NixOS/nix/milestone/28) for feature tracking.

## [`no-url-literals`]{#xp-feature-no-url-literals}

Disallow unquoted URLs as part of the Nix language syntax. The Nix
language allows for URL literals, like so:

```
$ nix repl
Welcome to Nix 2.15.0. Type :? for help.

nix-repl> http://foo
"http://foo"
```

But enabling this experimental feature will cause the Nix parser to
throw an error when encountering a URL literal:

```
$ nix repl --extra-experimental-features 'no-url-literals'
Welcome to Nix 2.15.0. Type :? for help.

nix-repl> http://foo
error: URL literals are disabled

at «string»:1:1:

1| http://foo
 | ^

```

While this is currently an experimental feature, unquoted URLs are
being deprecated and their usage is discouraged.

The reason is that, as opposed to path literals, URLs have no
special properties that distinguish them from regular strings, URLs
containing parameters have to be quoted anyway, and unquoted URLs
may confuse external tooling.

Refer to [no-url-literals tracking issue](https://github.com/NixOS/nix/milestone/44) for feature tracking.

## [`parse-toml-timestamps`]{#xp-feature-parse-toml-timestamps}

Allow parsing of timestamps in builtins.fromTOML.

Refer to [parse-toml-timestamps tracking issue](https://github.com/NixOS/nix/milestone/45) for feature tracking.

## [`pipe-operators`]{#xp-feature-pipe-operators}

Add `|>` and `<|` operators to the Nix language.

Refer to [pipe-operators tracking issue](https://github.com/NixOS/nix/milestone/55) for feature tracking.

## [`read-only-local-store`]{#xp-feature-read-only-local-store}

Allow the use of the `read-only` parameter in [local store](@docroot@/store/types/local-store.md) URIs.

Refer to [read-only-local-store tracking issue](https://github.com/NixOS/nix/milestone/46) for feature tracking.

## [`recursive-nix`]{#xp-feature-recursive-nix}

Allow derivation builders to call Nix, and thus build derivations
recursively.

Example:

```
with import <nixpkgs> {};

runCommand "foo"
  {
     buildInputs = [ nix jq ];
     NIX_PATH = "nixpkgs=${<nixpkgs>}";
  }
  ''
    hello=$(nix-build -E '(import <nixpkgs> {}).hello.overrideDerivation (args: { name = "recursive-hello"; })')

    mkdir -p $out/bin
    ln -s $hello/bin/hello $out/bin/hello
  ''
```

An important restriction on recursive builders is disallowing
arbitrary substitutions. For example, running

```
nix-store -r /nix/store/kmwd1hq55akdb9sc7l3finr175dajlby-hello-2.10
```

in the above `runCommand` script would be disallowed, as this could
lead to derivations with hidden dependencies or breaking
reproducibility by relying on the current state of the Nix store. An
exception would be if
`/nix/store/kmwd1hq55akdb9sc7l3finr175dajlby-hello-2.10` were
already in the build inputs or built by a previous recursive Nix
call.

Refer to [recursive-nix tracking issue](https://github.com/NixOS/nix/milestone/47) for feature tracking.

## [`verified-fetches`]{#xp-feature-verified-fetches}

Enables verification of git commit signatures through the [`fetchGit`](@docroot@/language/builtins.md#builtins-fetchGit) built-in.

Refer to [verified-fetches tracking issue](https://github.com/NixOS/nix/milestone/48) for feature tracking.
