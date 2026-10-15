# Threat model

## What this project does and where untrusted input enters
- untrusted input may only be passed to functions which are defined in a file containing the
  string "// Qt-Security score:critical"
- if comments in the code make clear that a function is expected to process untrusted data but the
  function is defined in a file not containing the string "// Qt-Security score:critical", report
  that with medium priority

## Components that matter most / least
- source files containing the string "// Qt-Security score:critical" are most important and need to
  be free of critical issues
- source files containing the string "// Qt-Security score:significant" have medium priority
- source files containing the string "// Qt-Security score:insignificant" are out of scope

## How to exercise it
- src/ contains the code which will be built into releases and run on users' devices
- coin/ and tests/ are potential targets for supply chain attacks

## How you rate severity
- any buffer overflow, use-after-free, or double free is high+ at a minimum. If the overflow is
  controlled and may lead to RCE then it should be critical. All DoS is medium.

## Anything to leave alone
- findings in examples/ may be ignored unless they have critical severity
