# janet-termsize

A Janet implementation of determining a terminal's size (rows,
columns).  No compilation necessary.

## Usage

```janet
(import ./janet-termsize :as ts)

(ts/termsize)
# =>
[24 80]
```

## Notes

Possibly of interest:

* The `:web` "operating system" (emscripten) isn't supported, but
  various attempts have been made to try to support others including:

  * `:bsd` - BSD that isn't the other specific ones?
  * `:cygwin`
  * `:dragonfly`
  * `:freebsd`
  * `:linux` - includes Termux on Android
  * `:macos`
  * `:mingw`
  * `:netbsd`
  * `:posix` - some UNIXy machine not more specifically recognized?
  * `:windows`

  Not all have been tested though.  Please report if something isn't
  working :)

* Tries to use FFI where practical (ATM, only x86_64 + some operating
  systems)

* Falls back to external process methods (`stty` for Unixy systems and
  powershell for Windows)

* The function `termsize` hands things off to various helper functions
  such as `via-ioctl`, `via-winapi`, or `via-shell`.  These are
  exposed but no particular effort may be made to keep them the same
  (or around) over time [1].

* When implementing FFI-related bits for Windows, it was helpful to
  examine [this
  page](https://learn.microsoft.com/en-us/windows/win32/winprog/windows-data-types)
  to figure out what things like `DWORD`, `HANDLE`, etc. are under the
  covers.  Once the underlying types were understood, [this
  table](https://janet-lang.org/docs/ffi.html#Primitive-Types) was
  used to determine a target keyword.  As an example, consider
  `DWORD`.  According to the page at MS, it is:

      > A 32-bit unsigned integer. The range is 0 through 4294967295
      > decimal.

  So taking that to mean `uint32_t`, according to the "primitive
  types" table, that could be either `:uint32` or `:u32` (both
  probably work).

---

[1] The author doesn't find making things private to be worth all of
    the extra work that often seems to lead to (e.g. testing and
    investigation typically become more cumbersome).

