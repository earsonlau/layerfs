Install LayerFS Workspace Setup {{version}} for me.

1. Clone https://github.com/Ephemeral-AI-Lab/layerfs beside this repository and check out commit {{layerfs.commit}} ({{layerfs.version}}). Use exactly this commit: it is the one this version was tested with. Change no code in that checkout.
2. Clone {{repository}} beside it and check out commit {{commit}}. Change no code in that checkout either.
3. Prerequisites: record `node --version` (need Node.js 20 or newer) and `npm --version`. If Node is missing or too old, stop and tell me.
4. In the plugin checkout, run `npm install`, then `node bin/layerfs-workspace-setup.js --help`. Confirm it prints usage for `apply`, `check`, and `init`.
5. Run `node bin/layerfs-workspace-setup.js --version` and confirm it prints {{version}}.
6. Smoke test in a temporary empty directory: `node /absolute/path/to/plugin/bin/layerfs-workspace-setup.js init`, then confirm `.layerfs/setup.json` exists and is valid JSON with a `copy` array. Delete the temporary directory when done. Do not run `apply` against a real project unless I ask.
7. Optionally run `npm test` in the plugin checkout; expect the suite to pass (51 tests at this pin).

Report what you ran and what each step returned. If anything fails, stop and give me the exact error, and don't retry it on your own.
