# Roblox Remote Guard

> A tiny, copy-paste-friendly guard for Roblox `RemoteEvent`s.

Remote Guard makes it easy to add three sensible protections to server events:

- **Rate limits** — slow down spammy calls.
- **Type checks** — reject malformed input early.
- **Clear responses** — keep gameplay code simple and readable.

It is deliberately dependency-free and small enough to understand in one sitting. Contributions, examples, and translations are welcome.

## Why this exists

Every client request should be treated as untrusted. New Roblox developers often know that in theory, but do not have a lightweight pattern for applying it. This project offers a friendly starting point—not a replacement for server-side game logic.

## Quick start

1. Copy `src/ServerScriptService/RemoteGuard.lua` into `ServerScriptService`.
2. Add the example server script and create a `RemoteEvent` named `PurchaseRequest` in `ReplicatedStorage`.
3. Adjust the validation function for your event.

```lua
local RemoteGuard = require(game.ServerScriptService.RemoteGuard)

local guard = RemoteGuard.new({
    calls = 5,
    perSeconds = 10,
})

PurchaseRequest.OnServerEvent:Connect(function(player, itemId)
    local allowed, reason = guard:check(player, itemId)
    if not allowed then
        warn(("Rejected request from %s: %s"):format(player.Name, reason))
        return
    end

    -- Look up the item and charge currency on the SERVER here.
end)
```

## What it checks

| Check | Default behavior |
| --- | --- |
| Player rate limit | 5 calls every 10 seconds |
| Argument validator | Optional function returning `true` or `false, reason` |
| Cleanup | Removes a player's stored request history when they leave |

## Project layout

```text
src/
├── ServerScriptService/
│   └── RemoteGuard.lua
└── examples/
    └── PurchaseRequest.server.lua
```

## Contributing

Keep pull requests focused, explain the gameplay problem being solved, and avoid adding dependencies. Good first contributions include a safe event example, test coverage, or clearer documentation.

## License

[MIT](LICENSE) — use it, adapt it, and ship something fun.
