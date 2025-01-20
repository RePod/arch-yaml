### [Individual Games](games/sync)

 I use this repo to manage individual YAMLs and combine them all into my GigaYAML™. Doing so manually would be way too much effort and error prone.

### ~~Quick~~ HOWTO
 - I don't recommend forking with this repo as upstream. Its commit history and syncing with are pointless for you.
   - Grab the [current main ZIP](https://github.com/RePod/arch-yaml/archive/refs/heads/main.zip) and make a fresh repo with it.
 - Edit `games/header.yaml` and `games/triggers.yaml`
   - Any instances of the name. Keep `{NUMBER}` everywhere as it prevents name conflicts if the same game is chosen.
   - Note the `async` and `per_game_name` triggers, these will be global.
 - Clear out the `games/custom_worlds/` folder unless you plan on playing a Manual Hatsune Miku Project Diva Mega Mix+!
 - Clear out the `games/sync/` folder, or keep some around as a template until you're finished.
   - The `description` property will be very important soon!
```YAML
# Simple YAML with triggers in place. For Official worlds, this format is all you'll need.
name: Name{NUMBER}
game: MYGAME
async: false
per_game_name: true

MYGAME:
  progression_balancing: random-low
  accessibility: random

triggers:
  - option_category: null
    option_name: async
    option_result: true
    options:
      MYGAME:
        progression_balancing: 0
  - option_name: game
    option_result: Game
    options:
      null:
        name: NameMYGAME{NUMBER}
  - option_name: per_game_name
    option_result: false
    options:
      null:
        name: Name{NUMBER}
```
 - Once published to GH, go to your repo **Settings** tab > **Actions** > **General**
   - Actions permissions > Allow all actions and reusable workflows
   - Workflow permissions > Read and write permissions (to update the GigaYAML at the root of the repo with a commit)
     - May change this to a Release in the future.
- The `.github/workflows/check.yml` Action will now run on push, PR, and weekly.
  - The Action pulls the latest version of Archipelago and described Unofficial worlds then runs test generations.
  - If the Action succeeds, the new GigaYAML is generated.
- For Unofficial worlds, the Action uses the YAML's `description` property that can point to a few places: (these examples may not exist anymore)
  - The Releases page, the Action will use the latest `.apworld` it can find. `https://github.com/ScipioWright/Archipelago-SW/releases/latest` (`/latest` optional)
  - URLs ending in `.apworld`: `https://github.com/ScipioWright/Archipelago-SW/releases/download/0.5.2/animal_well.apworld`
  - If there are any, refer to the original YAMLs in `games/sync/` for examples.
  - If all else fails, you can provide apworlds by putting them in `games/custom_worlds`.
