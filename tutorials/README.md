# Tutorials

Step-by-step walkthroughs with a complete worked example. Each one shows the exact lines to add, where to edit them in the panel, how to apply them (reload or client update) and how to test in game. For every column and option, follow the links to the reference pages.

| Tutorial | What you build |
|---|---|
| [Mini tutorial: configure an EventItemBag](event-item-bag-step-by-step.md) | A box opened by throwing it on the ground (normal model) and a boss drop with per-class rewards (advanced model), in the files and in the EventItemBag editor. |
| [Adding a custom item (CustomItem)](custom-item.md) | A new sword: server item row, client item entry, model registration, glow and effects, tooltip, shops and bags, publishing and testing with `/make`. |
| [Adding a custom map](custom-map.md) | A new hunting map: terrain, map rules, gate and warp, monsters, client world files, name, title image, M window entry and map markers. |
| [Adding a custom wing](custom-wing.md) | A new wing with its own defense, damage, absorption and options on both sides, model, glow, cape cloth and the 3rd-wing mix. |
| [Adding a custom pet](custom-pet.md) | A new pet with bonuses, movement style, model and sparkles, and how to turn it into a mount. |

{% hint style="info" %}
Most of these tutorials touch **both sides**. Server files are applied with a reload (or a GameServer restart for new items). Client custom files and client tables only reach players after you publish the update from the Client Hub ([Publishing updates](../dashboard/client-hub/publishing-updates.md)) and they restart the game.
{% endhint %}
