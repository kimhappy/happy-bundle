# happy-bundle
*happy-bundle* is a cmake-only C++23 library to bundle files as bytes.

## Example
```cmake
happy_bundle ( my_bundle
  HEADER_NAME my_files.hpp
  NAMESPACE   my::space
  FILES
    README.md
    asset/cat.png
    ${CMAKE_CURRENT_LIST_DIR}/.gitignore
)

target_link_libraries ( my_target
  PRIVATE
    my_bundle
)
```

```cpp
#include <my_files.hpp>

auto main() noexcept -> int {
  // Compile-time lookup: std::optional< std::span< std::byte const > >
  constexpr auto readme     = my::space::data(u8"README.md"            ); // Exists
  constexpr auto cat        = my::space::data(u8"cat.png"              ); // Exists
  constexpr auto impossible = my::space::data(u8"how_to_master_cpp.txt"); // Not exist

  static_assert( hi   .has_value());
  static_assert( hello.has_value());
  static_assert(!bye  .has_value());

  return 0;
}
```

## CMake Integration
```cmake
include ( FetchContent )

FetchContent_Declare ( happy_bundle
  GIT_REPOSITORY https://github.com/kimhappy/happy-bundle.git
  GIT_TAG        master
)

FetchContent_MakeAvailable ( happy_bundle )

# And make your bundle targets!
```
