# TODO

## `@pkmn/sim`

- Pokémon Showdown's underlying data definitions (eg. `MoveData`) could use a lot of cleanup and
  consolidation (better grouping, naming, etc)
- In an ideal world, `@pkmn/sim` could be made to depend on `@pkmn/dex`. The main blockers are:
  - `TeamValidator` needs to be able able to handle an `async getLearnsets` API (requires changes
  to Pokémon Showdown to avoid forking more files)
  - the `import` script needs to decompose Pokémon Showdown's 'data' files to write out just the
  handler logic which can be recombined with the data from `@pkmn/dex` on demand.

## `@pkmn/sets`

- a `canonicalize` method needs to be added to convert a `PokemonSet` into a canonical form. This
  includes (but is not limited to):
  - turning battle only formes into their base forme (with some more sophisticated logic for
    hackmons?)
  - removing EVs (and IVs?) which dont contribute additional stat points (note: this is more complex
    than just rounding to nearest 4)
  - encoding all IVs and EVs (including Gen 2 Marowak with Swords Dance and Thick Club, Hidden Power
    HP DVs, gender, shiny etc)
  - encoding happiness (handling Return/Frustration etc)
  - removing nicknames
  - moves sorted by ID
  - team members sorted by ID (how to handle formats without Species Clause?)

Once a stable format has been obtained, the team can be packed and then hashed, and then just the
hash can be shared to verify teams have not be changed (ie. **team lock**)

## `@pkmn/data`

- support O(1) lookup of data kinds by stable ID (requires efficient lookup to be implement in
  `@pkmn/sim` and `@pkmn/dex`)
- support iteration in stable ID order and avoid iterating over elements out of range (ie. only
  iterate over 151 objects for Gen 1 `Species` and then complete)

## `@pkmn/img`

- offload ugliness to `smogon/pokemon-sprites` (embed in `vendor/`?) related to gaps or missing data
  (should be symlinked), have `smogon/pokemon-sprites` build a mapping file that can be used to
  avoid redundant lookups
- handle full ordered preference array (not just `gen2g`, full array of preferences)
- allow for specifying whether to fall back to non-canonical or not

## `@pkmn/protocol`

- write tests...
- finish `Verifier` to be used by integration to comprehensively check correctness of Pokémon
  Showdown's protocol

## `@pkmn/login`

- zero dependency API for logging into Pokémon Showdown
- ideal: Pokémon Showdown provides OAuth support

## `@pkmn/view`

- demonstration of how to recreate Pokémon Showdown's animated battle hooks on top of `@pkmn/client`
- logic for displaying the Pokémon Showdown protocol *(how should arbitrary HTML be handled? Fully
  escaped? Or embed an HTML sanitizer?)*

## `@pkmn/randoms`

- add logic to `import` to extra all randoms battle *data* into `randoms/data` and logic into
  `random/src` (using similar approaches to how `@pkmn/dex` and `@pkmn/sim` are handled,
  respectively)
- encapsulate logic behind common `TeamGenerator` interface which can be wired into `@pkmn/sim`

## Integration

- Pokémon Showdown and `@pkmn` stacks in lockstep and compare (also wire in verifier from protocol
  and logs from view)