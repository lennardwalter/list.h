# list.h - A typesafe, generic, header-only doubly linked list implementation for C

-   [Usage](#usage)
    -   [Creating a list](#creating-a-list)
    -   [List operations](#list-operations)
    -   [Freeing the list](#freeing-the-list)
    -   [Example](#example)
-   [Compatibility](#compatibility)

## Usage

Include the header file

```c
#include "list.h"
```

### Creating a list

To create a list, you first need to declare its type. This can by done with `LIST_DEF`. You usually want to do this in a header file, but it even works within a function body. The first argument specifies the type of the list elements, and the second argument the type of the list itself.

```c
LIST_DEF(int, list_int_t);
```

You can then create a list of this type. Note that `LIST_NEW` returns a pointer to the (heap-allocated) list.

```c
list_int_t* list = LIST_NEW(list_int_t);
```

### List operations

You can now perform the following list operations. The first argument is always the list itself, so calling these functions looks like this: `LIST_APPEND(list, [arguments])`. In the following, `T` is used to denote the element type of the list.

| Name           | Description                                   | Arguments                   | Return |
| -------------- | --------------------------------------------- | --------------------------- | ------ |
| `LIST_APPEND`  | Add `element` to the end                      | `element: T`                | `void` |
| `LIST_PREPEND` | Add `element` to the beginning                | `element: T`                | `void` |
| `LIST_INSERT`  | Insert `element` at `index`                   | `index: size_t, element: T` | `void` |
| `LIST_GET`     | Retrieve the element at `index`               | `index: size_t`             | `T`    |
| `LIST_REMOVE`  | Remove the element at `index`                 | `index: size_t`             | `T`    |
| `LIST_POP`     | Retrieve the element at `index` and remove it | `index: size_t`             | `T`    |

To efficiently iterate through the list, use `LIST_FOREACH`/`LIST_FOREACH_REVERSE`

```c
LIST_FOREACH(list, el) {
    printf("%d\n", el);
}
```

### Freeing the list

The list is heap-allocated and thus needs to be freed. This can be done using `LIST_FREE(list)`. Keep in mind that each pointer in a list of pointers (to heap-allocated memory) needs to be freed seperately.

### Example

```c
#include <stdio.h>
#include "list.h"

LIST_DEF(int, list_int_t)

int main() {
    list_int_t* LIST_NEW(list_int_t);
    LIST_PREPEND(list, 1);
    LIST_INSERT(list, 1, 2);

    printf("list: ");

    LIST_FOREACH(list, el) {
        printf("%d ", el);
    }

    printf("\n");

    printf("second item: %d\n", LIST_GET(list, 1));

    LIST_FREE(list);

    return 0;
}
```

## Compatibility

This library uses a few compiler extensions which are not part of the C99 standard. The used extensions are `typeof()`, [Compound expressions](https://gcc.gnu.org/onlinedocs/gcc/Statement-Exprs.html) and `__COUNTER__`. It is tested on `gcc` and `clang` with `-std=gnu99`. Yyou don't this flag, the default works. However specifying `-std=c99` will cause compilation errors.
