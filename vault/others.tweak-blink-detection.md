---
id: ijCVN9lhTIBq3Ceo6XXTx
title: Tweak Blink Detection
desc: ''
updated: 1640018818426
created: 1640018789628
---

```
  +--------(eye open)--------+
  |                          |
  v                          |
OPEN ---(eye close)---> CLOSE_WAIT
  ^                          |
  |                 (eye still close)
(eye still open)             |
  |                          v
OPEN_WAIT <--(eye open)-- CLOSE
  |                          ^
  |                          |
  +-------(eye close)--------+
```

- Apply [[others.adaptive-threshold]]
- Track velocity to filter out untrustworthy data
- Add `WAIT` states for FSM
- Sync left/right eye by close/open both eye when all/one of them are in CLOSE_WAIT/OPEN_WAIT state (depends if ignore single blink)

