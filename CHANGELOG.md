# Changelog

## Committed Changes

### `internal/config/config.go`

- **Title agent `MaxTokens` increased** from `80` → `1024` (line 213). This allows the title agent to generate longer session titles.

### `internal/llm/models/local.go`

- **`listLocalModels` now supports authentication**: replaced plain `http.Get` with `http.NewRequest` + `http.DefaultClient.Do`, adding an `Authorization: Bearer <key>` header when `OPENAI_API_KEY` is set.

---

## Note: `opencode.json` vs `.opencode.json`

The file `opencode.json` (without dot prefix) in the repo root is **not used** by the config loading logic. The application uses **Viper** and looks for `.opencode.json` (with a dot prefix) only.

### Config file lookup order (Viper)

Viper searches for `.opencode.json` in the following paths, **first match wins**:

1. `$HOME/.opencode.json`
2. `$XDG_CONFIG_HOME/opencode/.opencode.json`
3. `$HOME/.config/opencode/.opencode.json`

Then, a **local** `.opencode.json` from the working directory is loaded and **merged** on top (local overrides global).

### Overall priority (highest → lowest)

1. `viper.Set()` calls (programmatic overrides)
2. Environment variables with `OPENCODE_` prefix (e.g. `OPENCODE_DEBUG=true`)
3. Local `.opencode.json` (working directory) — merged over global
4. Global `.opencode.json` (from paths above)
5. Defaults (`viper.SetDefault`)
