# Better Work Mode Stacking Cases

## Case 1: Project-Scale Task

Expected:

- activate `wave-based-delivery`
- do not force `round-based-execution` unless the current wave itself is locally complex

## Case 2: Project-Scale Task With Complex Current Wave

Expected:

- activate `wave-based-delivery`
- activate `round-based-execution` inside the current wave
- keep wave sequencing and round execution responsibilities separate

## Case 3: Complex But Not Project-Scale Task

Expected:

- activate `round-based-execution`
- do not activate `wave-based-delivery`

## Case 4: Small Bounded Task

Expected:

- stay in plain `better-work`
- avoid both wave mode and round mode
