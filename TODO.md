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

- consider rewriting based on
  [Pokémon Showdown](https://github.com/smogon/pokemon-showdown-client/blob/master/src/panel-teamdropdown.tsx)
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
hash can be shared to verify teams have not be changed (ie. **team lock**). Need to include a
version number in case set import changes.

## `@pkmn/img`

- redo API to use better identifiers than `gen2g` (`gen: 2` = `release: 'Crystal'` > `'gen2'`)
- handle full ordered preference array (not just `gen2g`, full array of preferences)
- allow for specifying whether to fall back to non-canonical or not**
- offload ugliness to `smogon/sprites` (embed in `vendor/`?) related to gaps or missing data
  (should be symlinked), have `smogon/sprites` build a mapping file that can be used to
  avoid redundant lookups

## `@pkmn/login`

- upkeep example with cookies

## `@pkmn/client`

- handle discrepancies with `@pkmn/sim` (for `@pkmn/epoke` state transforms)
  - change `type`/`addedType` etc handling to match server?

## `@pkmn/view`

- demonstration of how to recreate Pokémon Showdown's `AnimatedBattle` hooks on top of
  `@pkmn/client` (`BattleSceneStub` equivalent, but use `Partial` instead of null object pattern)
- logic for displaying the Pokémon Showdown protocol *(how should arbitrary HTML be handled? Fully
  escaped? Or embed an HTML sanitizer?)*
