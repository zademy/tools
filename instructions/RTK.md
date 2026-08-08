# RTK - Rust Token Killer

Token-optimized CLI proxy. Cuts up to 90% of bash output.

## Meta Commands

Always run these through `rtk` directly:

```bash
rtk gain              # Show token savings analytics
rtk gain --history    # Show command usage history with savings
rtk discover          # Analyze Claude Code history for missed opportunities
rtk proxy <cmd>       # Execute raw command without filtering (for debugging)
```

## Installation Verification

```bash
rtk --version         # Should show: rtk X.Y.Z
rtk gain              # Should work (not "command not found")
which rtk             # Verify correct binary
```

⚠️ **Name collision**: If `rtk gain` fails, you may have reachingforthejack/rtk (Rust Type Kit) installed instead.

## Hook-Based Usage

The Claude Code hook rewrites all other commands automatically: `git status` → `rtk git status` (transparent, 0 tokens overhead).

See `CLAUDE.md` for the full command reference.
