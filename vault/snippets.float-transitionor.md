---
id: yQm2b0QWjlr6u2y2dwvYO
title: Float Transitionor
desc: ''
updated: 1644732877394
created: 1644732690090
---
Transition a number for a period of time over a single loop call

## Implementation
```
class FloatTransitioner: NSObject {
    enum TransitionState {
        case idle, waiting, transitioning
    }

    private var state: TransitionState = .idle
    private var targetVal: Float = 0
    private var currentVal: Float = 0
    private var waitingDuration: Float = 0
    private var waitingDurationElapsed: Float = 0
    private var transitionIncrement: Float = 0
    private var finishCallback: (() -> Void)?

    func schedule(targetVal: Float,
                  waitingDuration: Float?,
                  transitionDuration: Float,
                  finishCallback: (() -> Void)?) {
        if self.currentVal == targetVal {
            state = .idle
            finishCallback?()
            return
        }

        if transitionDuration <= 0 { return }

        self.targetVal = targetVal
        self.transitionIncrement = (targetVal - self.currentVal) / transitionDuration
        self.finishCallback = finishCallback
        self.state = .transitioning

        if let waitingInterval = waitingDuration {
            if waitingInterval > 0 {
                self.waitingDuration = waitingInterval
                self.waitingDurationElapsed = 0
                state = .waiting
            }
        }
    }

    func update(_ deltaTime: Float) -> Float? {
        switch state {
        case .idle:
            return nil
        case .waiting:
            waitingDurationElapsed += deltaTime
            if waitingDurationElapsed >= waitingDuration {
                state = .transitioning
            }
            return currentVal
        case .transitioning:
            currentVal += deltaTime * transitionIncrement
            if sign(transitionIncrement) * (currentVal - targetVal) > 1e-5 {
                currentVal = targetVal
                state = .idle
                self.finishCallback?()
            }
            return currentVal
        }
    }

    func release() {
        self.state = .idle
        self.finishCallback = nil
    }
}

```