# Python Style Guide

*A mostly reasonable approach to Python*

This is a modern Python style guide born from numerous open source projects and collaborative discussions. You can use this concise guide on its own or with PEP8. This guide focuses on the cutting edge of Python 3.

A style guide must constantly adapt to newer norms, and its basis is community. If you find this useful, please share. If you have updates to contribute, please submit a PR so that the community can adopt them.

Other Style Guides

  - [JavaScript](https://github.com/airbnb/javascript)
  - [Ruby](https://github.com/airbnb/ruby)

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Dictionaries](#dictionaries)
  1. [Lists](#lists)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Variables](#variables)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Naming Conventions](#naming-conventions)
  1. [Testing](#testing)
  1. [Resources](#resources)
  1. [Contributors](#contributors)

## Types

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Primitives**: When you access a primitive type you work directly on its value.

    - `string`
    - `number`
    - `boolean`
    - `None`

    ```python
    foo = 1
    bar = foo

    bar = 9

    print(foo, bar) # => 1, 9
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **Complex**: When you access a complex type you work on a reference to its value.

    - `dict`
    - `list`
    - `function`

    ```python
    foo = [1, 2]
    bar = foo

    bar[0] = 9

    print(foo[0], bar[0]) # => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Use `CONST` for all of your references; avoid using `var`. Python does not have `constant` type, so you need to observe the convention using UPPERCASE for constants and never modify them.

    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```python
    # bad
    foo = 1
    bar = 2

    # good
    FOO = 1
    BAR = 2
    ```

**[⬆ back to top](#table-of-contents)**

## Dictionaries

  <a name="dictionaries--literals"></a><a name="3.1"></a>
  - [3.1](#dictionaries--literals) Use the literal syntax for dictionary creation.

    ```python
    # bad
    item = dict()

    # good
    item = {}
    ```

  <a name="computed-key"></a><a name="3.2"></a>
  - [3.2](#computed-key) Use computed key names when creating dictionaries with dynamic key names.

    > Why? They allow you to define all the key of a dictionary in one place.

    ```python

    def get_key(k):
        return f'a key named {k}'

    # bad
    obj = {
        'id': 5,
        'name': 'San Francisco',
    }
    obj[get_key('enabled')] = True

    # good
    obj = {
        'id': 5,
        'name': 'San Francisco',
        get_key('enabled'): True,
    }
    ```

  <a name="dictionaries--spread"></a><a name="3.3"></a>
  - [3.3](#dictionaries--spread) Prefer the dictionary spread operator over `copy()` to shallow-copy and extend dictionaries.

    ```python
    # bad
    original = {'a': 1, 'b': 2}
    clone = original.copy()
    clone.update({'c': 3})

    # good
    original = {'a': 1, 'b': 2}
    clone = {**original, 'c': 3}

    # good
    original = {'a': 1, 'b': 2}
    original_2 = {'c': 3, 'd': 4}
    long_clone = {**original, **original_2, 'e': 5}
    ```

  <a name="dictionaries--braces-newline"></a><a name="3.4"></a>
  - [3.4](#dictionaries--braces-newline) Use line breaks after open and before close dictionary braces only if a dictionary has multiple lines.

    ```python
    # bad - single item will not exceed one line
    single_map = {
        'a': 1,
    }

    # bad - single line
    item_map = {
        'a': 1, 'b': 2, 'c': 3,
    }

    # good
    single_map = {'a': 1}

    item_map = {
        'a': 1,
        'b': 2,
        'c': 3,
    }
    ```

  <a name="dictionaries--use-get"></a><a name="3.5"></a>
  - [3.5](#dictionaries--use-get) Use `dict.get(key)` to get properties.

    > Why? Getting via `dict[key]` will break on missing key, and requires bloated code to guard against.

    ```python
    item_map = {
        'a': 1,
        'b': 2,
    }

    # bad - throws error
    item_map['c']

    # bad - bloated code
    try:
        item_map['c']
    except KeyError:
        item_map['c'] = 3
        return item_map['c']

    # good
    item_map.get('c')

    # good
    item_map['c'] = item_map.get('c') or 3
    ```

**[⬆ back to top](#table-of-contents)**

## Lists

  <a name="lists--literals"></a><a name="4.1"></a>
  - [4.1](#lists--literals) Use the literal syntax for list creation.

    ```python
    # bad
    items = list()

    # good
    items = []
    ```

  <a name="list--spreads"></a><a name="4.2"></a>
  - [4.2](#list--spreads) Use list spreads `*` to copy and extend lists.

    ```python
    # bad
    items = ['a', 'b']
    clone = items.copy() + ['c']

    # good
    items = ['a', 'b']
    clone = [*items, 'c']

    # good
    items = ['a', 'b']
    items_2 = ['c', 'd']
    clone = [*items, *items2, 'e']
    ```

  <a name="lists--bracket-newline"></a><a name="4.3"></a>
  - [4.3](#lists--bracket-newline) Use line breaks after open and before close list brackets only if a list has multiple lines.

    ```python
    # bad - single line
    items = [
        [0, 1], [2, 3], [4, 5],
    ]

    # bad - no line break after bracket
    dict_list = [{
        'id': 1
    }, {
        'id': 2
    }]

    number_list = [
        1, 2,
    ]

    # good
    items = [[0, 1], [2, 3], [4, 5]]

    dict_list = [
        {'id': 1},
        {'id': 2},
    ]

    number_list = [
        1,
        2,
    ]
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  <a name="destructuring--list"></a><a name="5.1"></a>
  - [5.1](#destructuring--list) Use list destructuring.

    ```python
    items = [1, 2, 3, 4, 5]

    # bad
    first = items[0]
    second = items[1]

    # good
    first, second, *tail = items
    first, second, *rest, last = items
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  <a name="stringss--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) Use single quotes `''` for strings.

    > Why? Less escaping for double quote `""`, less bloat, and makes code more searchable.

    ```python
    # bad
    name = "Capt. Janeway"

    # bad
    json_string = "{\"a\": 1}"

    # bad - f string should contain interpolation or newlines
    name = f'Capt. Janeway'

    # good
    name = 'Capt. Janeway'

    # good
    json_string = '{"a": 1}'
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Strings that cause the line to go over 79 characters should not be written across multiple lines using string concatenation.

    > Why? Broken strings are painful to work with and make code less searchable.

    ```python
    # bad
    error_msg = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.'

    # bad
    error_msg = 'This is a super long error that was thrown because ' + \
        'of Batman. When you stop to think about how Batman had anything to do ' + \
        'with this, you would get nowhere fast.'

    # good
    error_msg = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'
    ```

  <a name="template-literals"></a><a name="6.3"></a>
  - [6.3](#template-literals) When programmatically building up strings, use template strings instead of concatenation.

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```python
    # bad
    def say_hi(name):
        return 'How are you, ' + name + '?'

    # bad
    def say_hi(name):
        return ''.join(['How are you, ', name, '?'])

    # bad
    def say_hi(name):
        return f'How are you, { name }?'

    # good
    def say_hi(name):
        return f'How are you, {name}?'
    ```

    > For python < 3.6, convert f-string to template string by `format()`:

    ```python
    a = 1
    b = 2
    c = 3

    # python >=3.6
    f'a: {a} b: {b} c: {c}'

    # python <3.6
    'a: {a} b: {b} c: {c}'.format(a=a, b=b, c=c)

    param = {'a': a, 'b': b, 'c': c}
    'a: {a} b: {b} c: {c}'.format(**param)

    'a: {} b: {} c: {}'.format(a, b, c)
    ```

  <a name="strings--eval"></a><a name="6.4"></a>
  - [6.4](#strings--eval) Never use `eval()` on a string, it opens too many vulnerabilities.

  <a name="strings--escaping"></a><a name="6.5"></a>
  - [6.5](#strings--escaping) Do not unnecessarily escape characters in strings.

    > Why? Backslashes harm readability, thus they should only be present when necessary.

    ```python
    # bad
    foo = '\'this\' \i\s \"quoted\"'

    # good
    foo = '\'this\' is "quoted"'
    foo = f'my name is "{name}"'
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  <a name="default-parameters"></a><a name="7.1"></a>
  - [7.1](#default-parameters) Use default parameter syntax rather than mutating function arguments.

    ```python
    # really bad
    def do_something(opt):
        # No! We shouldn’t mutate function arguments.
        # Double bad: if opt is falsy it'll be set to an object which may
        # be what you want but it can introduce subtle bugs.
        opt = opt or 'foo'
        # ...

    # still bad
    def do_something(opt):
        if (opt is None):
            opt = 'foo'
        # ...

    # good
    def do_something(opt='foo'):
        # ...
    ```

  <a name="no-complex-default-parameters"></a><a name="7.2"></a>
  - [7.2](#no-complex-default-parameters) Do not use complex data type as default parameter.

    > Why? Variable to a complex type is a reference, and so the single instance will be modified.

    ```python
    # bad
    def init_list(value, new_list=[]):
        new_list.append(value)
        return new_list

    init_list(1)
    # => [1]
    init_list(2)
    # => [1, 2], instead of the new init [2]

    # good
    def init_list(value, new_list=None):
        if new_list is None:
            new_list = []
        new_list.append(value)
        return new_list

    init_list(1)
    # => [1]
    init_list(2)
    # => [2]
    ```

  <a name="functions--signature-no-spacing"></a><a name="7.3"></a>
  - [7.3](#functions--signature-no-spacing) No spacing in a function signature.

    > Why? Consistency is good, and eases code search.

    ```python
    # bad
    def foo(a): print(a)
    def bar (b): print(b)

    # good
    def foo(a): print(a)
    def bar(b): print(b)
    ```

  <a name="functions--reassign-params"></a><a name="7.4"></a>
  - [7.4](#functions--reassign-params) Never reassign parameters.

    > Why? Reassigning parameters can lead to unexpected behavior.

    ```python
    # bad
    def fn_1(a):
        a = 1
        # ...

    def fn_2(a):
        if (!a): a = 1
        # ...

    # good
    def fn_3(a):
        b = a or 1
        # ...

    def fn_4(a=1):
        # ...
    ```

  <a name="functions--signature-invocation-indentation"></a><a name="7.5"></a>
  - [7.5](#functions--signature-invocation-indentation) Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, with a trailing comma on the last item.

    ```python
    # bad
    def some_fn(foo,
                bar,
                baz):
        # ...

    # good
    def some_fn(
        foo,
        bar,
        baz,
    ):
        # ...

    # bad
    some_fn(foo,
      bar,
      baz)

    # good
    some_fn(
      foo,
      bar,
      baz,
    )
    ```

  <a name="functions--call-param-name"></a><a name="7.6"></a>
  - [7.6](#functions--call-param-name) Call function with parameters by specifying their names.

    > Why? Clarity of parameters and future-proofing. When updating source code function parameters, it can be done reliably with minimal propagation.

    ```python
    def move(x, y, roll=False):
        # ...

    # bad - unclear what the params mean
    move(1, 0, True)

    # good
    move(x=1, y=0, roll=True)

    # later when updating method, no need to propagate function calls since they will auto-assume z=0 reliably
    def move(x, y, z=0, roll=False):
        # ...
    ```

  <a name="functions--clear-logic"></a><a name="7.7"></a>
  - [7.7](#functions--clear-logic) Break down code logic into digestible chunks, and refactor a lot.

    > Why? Other programmers and your future self will thank you for writing understandable code.

    ```python
    # bad - short but extremely confusing
    def long_logic():
        return [a for a in small_list for small_list in large_matrix if len(small_list) else ['replacement'] if len(a) > 2]

    # good - longer but very clear
    def long_logic():
        result = []
        for small_list in large_matrix:
            if len(small_list) > 0:
                used_list = small_list
            else:
                used_list = ['replacement']
            for a in used_list:
                if len(a) > 2:
                    result.append(a)
        return result
    ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

  <a name="classes--no-duplicate-members"></a><a name="8.1"></a>
  - [8.1](#classes--no-duplicate-members) Avoid duplicate class members.

    > Why? Duplicate class member declarations will silently prefer the last one - having duplicates is almost certainly a bug.

    ```python
    # bad
    class Foo():
        def bar(): return 1
        def bar(): return 2

    # good
    class Foo():
        def bar(): return 1
    ```

**[⬆ back to top](#table-of-contents)**

## Modules

  <a name="modules--no-wildcard"></a><a name="9.1"></a>
  - [9.1](#modules--no-wildcard) Do not use wildcard imports.

    > Why? To prevent namespace pollution and conflicts, and to know which modules your variables or functions come from.

    ```python
    # bad
    from common.util import *

    # good
    from common import util
    ```


  <a name="modules--no-unused"></a><a name="9.2"></a>
  - [9.2](#modules--no-unused) Do not import unused modules.
    > Why? Performance, reliability, containment. If a module breaks, your code that should be isolated from the module will break too. This causes more errors and makes it harder to debug.

  <a name="modules--no-duplicate-imports"></a><a name="9.3"></a>
  - [9.3](#modules--no-duplicate-imports) Only import from a path in one place.
    > Why? Having multiple lines that import from the same path can make code harder to maintain.

    ```python
    # bad
    from foo import bar
    # ... some other imports
    from foo import baz, qux

    # good
    from foo import bar, baz, qux

    # good
    from foo import (
        bar,
        baz,
        qux,
    )
    ```

  <a name="modules--imports-first"></a><a name="9.4"></a>
  - [9.4](#modules--imports-first) Put all `import`s above non-import statements.
    > Why? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

    ```python
    # bad
    import a_module
    from b_module import foo
    foo.init()

    from c_module import bar

    # good
    import a_module
    from b_module import foo
    from c_module import bar

    foo.init();
    ```

  <a name="modules--imports-sorted"></a><a name="9.5"></a>
  - [9.5](#modules--imports-sorted) Sort the `import`s by `import` then `from`, and sort alphabetically.
    > Why? `import` are often more generic that `from`; sort to ease manual inspection and for maintainability.

    ```python
    # bad
    from a_module import foo
    import e_module
    import b_module
    from c_module import c_fn, b_fn

    # good
    import b_module
    import e_module
    from a_module import foo
    from c_module import b_fn, c_fn
    ```

  <a name="modules--multiline-imports-over-newlines"></a><a name="9.6"></a>
  - [9.6](#modules--multiline-imports-over-newlines) Multiline imports should be indented just like multiline list and dictionary literals.

    > Why? The parentheses follow the same indentation rules as every other bracket or brace block in the style guide, as do the trailing commas.

    ```python
    # bad
    from a_module import long_name_a, long_name_b, long_name_c, long_name_d

    # good
    from a_module import (
      long_name_a,
      long_name_b,
      long_name_c,
      long_name_d,
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  (Pending)

**[⬆ back to top](#table-of-contents)**

## Variables

  <a name="variables--const"></a><a name="11.1"></a>
  - [11.1](#variables--const) Use UPPERCASE to declare constants, and observe the convention - do not modify them in the program. Python has no `constant` type, so it must be observed manually.

    ```python
    # bad
    a_constant = 1
    os.environ['py_env'] = 'development'

    # good
    A_CONSTANT = 1
    os.environ['PY_ENV'] = 'development'
    ```

  <a name="variables--one-const"></a><a name="11.2"></a>
  - [11.2](#variables--one-const) Declare one constant per line.

    > Why? For clarity, and it’s easier to add/remove declarations this way, and with minimal git-diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

    ```python
    # bad
    FOO, BAR, BAZ = 1, 2, 3

    # good
    FOO = 1
    BAR = 2
    BAZ = 3
    ```

  <a name="variables--const-group"></a><a name="11.3"></a>
  - [11.3](#variables--const-group) Group all your `CONST`s and then group all your `var`s.

    > Why? For clarity and ease of reference. This is also helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```python
    # bad
    FOO = 1
    counter = 0
    BAR = 2
    length = counter

    # good
    FOO = 1
    BAR = 2

    counter = 0
    length = counter
    ```

  <a name="variables--define-where-used"></a><a name="11.4"></a>
  - [11.4](#variables--define-where-used) Assign variables with the minimally sufficient scope at where you need them, but place them in a reasonable place.

    > Why? Prevent variable scope-leak and conflicts

    ```python
    # bad - leak to sibling
    counter = 0 # mean to count group_a only
    for list_a in group_a:
        counter += len(list_a)

    for list_b in group_b:
        counter += len(list_b)

    # bad - leak into smaller scope
    counter = 0 # mean to count within groups
    for group in super_group:
        counter += len(group)
        for list in group:
            counter += len(list)

    # good
    counter_a = 0
    for list_a in group_a:
        counter_a += len(list_a)

    counter_b = 0
    for list_b in group_b:
        counter_b += len(list_b)

    # good
    group_counter = 0 # mean to count within groups
    for group in super_group:
        group_counter += len(group)
        list_counter = 0 # mean to count within lists
        for list in group:
            list_counter += len(list)
    ```

  <a name="variables--underscore-unused"></a><a name="11.5"></a>
  - [11.5](#variables--underscore-unused) Prepend underscore `_` when naming variables that are unused. Also a part of PEP8.

    > Why? To be aware of data usage and side effects.

    ```python
    # bad
    first, unused, last = [1, 2, 3]

    # bad - finder is not used though expected to be
    for finder, replacer in some_map.items():
        do_something_without_key(replacer)

    # bad - lose track of what the first key is
    for _, replacer in some_map.items():
        do_something_without_key(replacer)

    # good
    first, _unused, last = [1, 2, 3]

    # good - we know what the variable is, and it is unused
    for _finder, replacer in some_map.items():
        do_something_without_key(replacer)
    ```

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  
  <a name="comparison--concise"></a><a name="12.1"></a>
  - [12.1](#comparison--concise) Use concise boolean conditionals, refactor long compound statements.

    > Why? Long boolean statements are hard to read and understand.

    ```python
    # bad
    if (can_move_x() and can_move_y() or is_light() or is_dry() or has_high_drag()):
        execute_operation_tumbleweed()
    else:
        execute_operation_cactus()

    # good
    can_move = can_move_x() and can_move_y()
    movable = is_light() or is_dry() or has_high_drag()
    if (can_move or movable):
        execute_operation_tumbleweed()
    else:
        execute_operation_cactus()
    ```

  <a name="comparison--direct"></a><a name="12.2"></a>
  - [12.2](#comparison--direct) Be direct with booleans, avoid unnecessary negations.

    > Why? Negations are harder to understand and longer to write.

    ```python
    # bad
    if not a_is_legal():
        do_b()
    else:
        do_a()

    # good
    if a_is_legal():
        do_a()
    else:
        do_b()
    ```

  <a name="comparison--shortcuts"></a><a name="12.3"></a>
  - [12.3](#comparison--shortcuts) Use shortcuts for booleans, but explicit comparisons for strings and numbers.

    ```python
    # bad
    if is_valid == true:
        # ...

    # good
    if is_valid:
        # ...

    # bad
    if name:
        # ...

    # good
    if name != '':
        # ...

    # bad
    if len(a_list):
        # ...

    # good
    if len(a_list) > 0:
        # ...
    ```

  <a name="comparison--nested-ternaries"></a><a name="12.4"></a>
  - [12.4](#comparison--nested-ternaries) Ternaries should not be nested and generally be single line expressions.

    ```python
    # bad
    foo = 'bar' if maybe_1 > maybe_2 else 'baz' if value_1 > value_2 else None

    # best
    maybe_none = 'baz' if value1 > value2 else None
    foo = 'bar' if maybe1 > maybe2 else maybe_none
    ```

  <a name="comparison--unneeded-ternary"></a><a name="12.5"></a>
  - [12.5](#comparison--unneeded-ternary) Avoid unneeded ternary statements.

    ```python
    # bad
    foo = a if a else b
    bar = True if c else False
    baz = False if c else True

    # good
    foo = a or b
    bar = c
    baz = not c
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments--multiline"></a><a name="13.1"></a>
  - [13.1](#comments--multiline) Use `'''...'''` for multi-line comments.

    ```python
    # bad
    def make(tag):
        # make() returns a new element
        # based on the passed in tag name
        #
        # @param {string} tag
        # @return {Element} element

        # ...
        return element

    # good
    def make(tag):
        '''
        make() returns a new element
        based on the passed in tag name
        
        @param {string} tag
        @return {Element} element
        '''

        # ...
        return element
    ```

  <a name="comments--singleline"></a><a name="13.2"></a>
  - [13.2](#comments--singleline) Use `#` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

    ```python
    # bad
    active = True  # is current tab

    # good
    # is current tab
    active = True

    # bad
    def get_type():
        print('fetching type...')
        # set the default type to 'no type'
        type = self.type or 'no type'

        return type

    # good
    def get_type():
        print('fetching type...')

        # set the default type to 'no type'
        type = self.type or 'no type'

        return type

    # also good
    def get_type():
        # set the default type to 'no type'
        type = self.type or 'no type'

        return type
    ```

  <a name="comments--spaces"></a><a name="13.3"></a>
  - [13.3](#comments--spaces) Start all comments with a space to make it easier to read.

    ```python
    # bad
    #is current tab
    active = True

    # good
    # is current tab
    active = True
    ```

  <a name="comments--actionitems"></a><a name="13.4"></a>
  - [13.4](#comments--actionitems) Prefixing your comments with `TODO` helps yourself and other developers be aware of items to revisit or implement. It keeps the issues visible and easy to find. These are different than regular comments because they are actionable.

    ```python
    def complex_calculator():
        compute_basic()

        # TODO figure out improvements to the logic
        return compute_core_logic()
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  <a name="whitespace--spaces"></a><a name="14.1"></a>
  - [14.1](#whitespace--spaces) Use soft tabs (space character) set to 4 spaces as per PEP8.

    ```python
    # bad
    def foo():
    ∙∙return bar

    # bad
    def foo():
    ∙return bar

    # good
    def foo():
    ∙∙∙∙return bar
    ```

  <a name="whitespace--infix-ops"></a><a name="14.2"></a>
  - [14.2](#whitespace--infix-ops) Set off operators with spaces.

    ```python
    # bad
    x=y+5

    # good
    x = y + 5
    ```

  <a name="whitespace--newline-at-end"></a><a name="14.3"></a>
  - [14.3](#whitespace--newline-at-end) End files with a single newline character.

    ```python
    # bad
    import util
    # ...
    def foo():
        return bar
    ```

    ```python
    # bad
    import util
    # ...
    def foo():
        return bar↵
    ↵
    ```

    ```python
    # bad
    import util
    # ...
    def foo():
        return bar↵
    ```

  <a name="whitespace--after-blocks"></a><a name="14.4"></a>
  - [14.4](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement.

    ```python
    # bad
    if foo:
        return bar
    return baz

    # good
    if foo:
        return bar

    return baz

    # bad
    results = [
        fn_1(),
        fn_2(),
    ]
    return results

    # good
    results = [
        fn_1(),
        fn_2(),
    ]

    return results
    ```

  <a name="whitespace--padded-blocks"></a><a name="14.5"></a>
  - [14.5](#whitespace--padded-blocks) Do not pad your blocks with blank lines.

    ```python
    # bad
    def bar():

        print(foo)

    # bad
    if (baz):

        print(qux)
    else:
        print(foo)

    # good
    def bar():
        print(foo)

    # good
    if (baz):
        print(qux)
    else:
        print(foo)
    ```

  <a name="whitespace--in-parens"></a><a name="14.6"></a>
  - [14.6](#whitespace--in-parens) Do not add spaces inside parentheses.

    ```python
    # bad
    def bar( foo ):
        return foo

    # good
    def bar(foo):
        return foo

    # bad
    if ( foo and fux ):
        print(foo)

    # good
    if (foo and fux):
        print(foo)
    ```

  <a name="whitespace--in-brackets"></a><a name="14.7"></a>
  - [14.7](#whitespace--in-brackets) Do not add spaces inside brackets or braces.

    ```python
    # bad
    foo = [ 1, 2, 3 ]
    print(foo[ 0 ])
    bar = { 'a': 1 }

    # good
    foo = [1, 2, 3]
    print(foo[0])
    bar = {'a': 1}
    ```

  <a name="whitespace--max-len"></a><a name="14.8"></a>
  - [14.8](#whitespace--max-len) Avoid having lines of code that are longer than 79 characters (including whitespace) as per PEP8. Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up.

    > Why? This ensures readability and maintainability.

    ```python
    # bad
    foo = nested_object and nested_object.foo and nested_object.foo.bar and nested_object.foo.bar.baz and nested_object.foo.bar.baz.quux and nested_object.foo.bar.baz.quux.xyzzy

    # bad
    http_call({'method': 'POST', 'url': 'https://airbnb.com/', 'data': {name: 'John', 'age': 20}})

    # good
    foo = (nested_object and
        nested_object.foo and
        nested_object.foo.bar and
        nested_object.foo.bar.baz and
        nested_object.foo.bar.baz.quux and
        nested_object.foo.bar.baz.quux.xyzzy)

    # good
    http_call({
        'method': 'POST',
        'url': 'https://airbnb.com/',
        'data': {name: 'John', 'age': 20},
    })
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  <a name="commas--leading-trailing"></a><a name="15.1"></a>
  - [15.1](#commas--leading-trailing) Leading commas: **Nope.**

    ```python
    # bad
    story = [
          once
        , upon
        , a_time
    ]

    # good
    story = [
        once,
        upon,
        a_time,
    ]

    # bad
    hero = {
        'first_name': 'Ada'
        , 'last_name': 'Lovelace'
        , 'birth_year': 1815
        , 'super_power': 'computers'
    }

    # good
    hero = {
        'first_name': 'Ada',
        'last_name': 'Lovelace',
        'birth_year': 1815,
        'super_power': 'computers',
    }
    ```

  <a name="commas--dangling"></a><a name="15.2"></a>
  - [15.2](#commas--dangling) Additional trailing comma: **Yup.**

    > Why? This leads to cleaner git diffs during code change.

    ```diff
    # bad - git diff without trailing comma
    hero = {
         'first_name': 'Ada',
    -    'last_name': 'Lovelace'
    +    'last_name': 'Lovelace',
    +    'super_power': 'computers',
    }

    # good - git diff with trailing comma
    hero = {
         'first_name': 'Ada',
         'last_name': 'Lovelace',
    +    'super_power': 'computers',
    }
    ```

    ```python
    # bad
    hero = {
        'first_name': 'Ada',
        'last_name': 'Lovelace'
    }

    heroes = [
        'Batman',
        'Superman'
    ]

    # good
    hero = {
        'first_name': 'Ada',
        'last_name': 'Lovelace',
    }

    heroes = [
        'Batman',
        'Superman',
    ]

    # bad
    def create_hero(
        first_name,
        last_name,
        superpower
    ):
        # ...

    # good
    def create_hero(
        first_name,
        last_name,
        superpower,
    ):
        # ...

    # good - note that a comma must not appear after a "spread" element
    def create_hero(
        first_name,
        last_name,
        superpower,
        **kwargs
    ):
        # ...

    # bad
    create_hero(
        first_name,
        last_name,
        superpower
    )

    # good
    create_hero(
        first_name,
        last_name,
        superpower,
    )

    # good - note that a comma must not appear after a "spread" element
    create_hero(
        first_name,
        last_name,
        superpower,
        **kwargs
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--snake_case"></a><a name="16.1"></a>
  - [16.1](#naming--snake_case) Use snake_case when naming variables, functions, and instances. Use it for file names too as they will be used in imports.

    ```python
    # bad
    import myModule
    OBJEcttsssss = {}
    thisIsMyObject = {}
    def thisIsMyFunction():

    # good
    import my_module
    objects = {}
    this_is_my_object = {}
    def this_is_my_function():
    ```

  <a name="naming--PascalCase"></a><a name="16.2"></a>
  - [16.2](#naming--PascalCase) Use PascalCase only when naming classes.

    ```python
    # bad
    class prioritizedMemoryReplay():
        # ...

    memory = prioritizedMemoryReplay()

    # good
    class PrioritizedMemoryReplay():
        # ...

    memory = PrioritizedMemoryReplay()
    ```

  <a name="naming--descriptive"></a><a name="16.3"></a>
  - [16.3](#naming--descriptive) Avoid single letter names. Use descriptive and meaningful names - tell what the function does, or what data type an object is. Use `description_object` instead of `object_description`.

    ```python
    # bad
    def q():
        # ...

    # good
    def query():
        # ...

    # bad - no convention to know what data type it is
    df_raw_data = pd.DataFrame(some_data)
    id_map_num = {'a': 1, 'b': 2}

    # good - convention to tell data type by the last term
    raw_data_df = pd.DataFrame(some_data)
    id_num_map = {'a': 1, 'b': 2}

    # bad - meaningless names, lost context
    LIST_1 = ['Jack', 'Alice', 'Emily']
    # ... many lines of code later
    for item in LIST_1:
        register_human(item)

    # good
    NAME_LIST = ['Jack', 'Alice', 'Emily']
    # ... many lines of code later
    for name in NAME_LIST:
        register_human(name)
    ```

  <a name="naming--distance"></a><a name="16.4"></a>
  - [16.4](#naming--distance) Avoid using close naming to prevent typo and confusion.

    ```python
    # bad
    objects = ['rock', 'paper', 'scissors']
    for object in objects:
        register_item(object)
    close_box(objects)

    # okay
    objects = ['rock', 'paper', 'scissors']
    for obj in objects:
        register_item(obj)
    close_box(objects)

    # best
    object_list = ['rock', 'paper', 'scissors']
    for object in object_list:
        register_item(object)
    close_box(object_list)
    ```

  <a name="naming--singular"></a><a name="16.5"></a>
  - [16.5](#naming--singular) Use singular or base words in naming; avoid using plural and instead append singular with the data type.

    > Why? To prevent inconsistencies and second-guesses when using variables. Also, plurals are 1 letter away from a typo, are hard to read, and are ambiguous on the data type.

    ```python
    # bad
    def moves_object(x, y):
        # ...

    # good
    def move_object(x, y):
        # ...

    # bad - inconsistent naming for same data type and usage
    teacher = ['Michael']
    students = ['Jack', 'Alice', 'Emily']
    books = pd.DataFrame({'title': ['lorem', 'ipsum']})

    for t in teacher:
        register_human(t)

    for student in students:
        register_human(student)

    for book in books:
        register_item(book) # wrong; iterate column name instead of book

    # good
    teacher_list = ['Michael']
    student_list = ['Jack', 'Alice', 'Emily']
    book_df = pd.DataFrame({'title': ['lorem', 'ipsum']})

    for teacher in teacher_list:
        register_human(teacher)

    for student in student_list:
        register_human(student)

    # naming as df suggests it shall be treated as a dataframe
    for _idx, book in book_df.iterrow():
        register_item(book)
    ```

  <a name="naming--singular-module"></a><a name="16.6"></a>
  - [16.6](#naming--singular-module) Use singular naming for modules and source files.

    ```python
    # bad
    from commons import utils

    utils.read_string()

    # good
    from common import util

    util.read_string()
    ```

  <a name="naming--abbreviations"></a><a name="16.7"></a>
  - [16.7](#naming--abbreviations) Use abbreviations if they are clear and make for more readable and writable code.

    > Why? Names are for humans, so always make code readable and easy to spell.

    ```python
    # bad
    flight_prerequisites_checklist = ['landing gear', 'engine', 'flaps']
    initialize_flight(flight_prerequisites_checklist)

    # good
    flight_prereq_checklist = ['landing gear', 'engine', 'flaps']
    init_flight(flight_prereq_checklist)
    ```

  <a name="naming--short"></a><a name="16.8"></a>
  - [16.8](#naming--short) Use simple, concise names over long, explicit ones.

    > Why? Names are for humans to read, and should make the code clean.

    ```python
    # bad - explicit Java-style naming clutters code and harms readability
    poscode_to_city_name_map = {11223: 'brooklyn'}
    poscode_to_city_name_to_state_name_map = map_city_to_state(poscode_to_city_name_map)
    poscode_to_city_name_to_state_name_to_country_map = {}

    # good - understandable and fast to read
    poscode_city_map = {11223: 'brooklyn'}
    poscode_state_map = map_city_to_state(poscode_city_map)
    poscode_country_map = {}
    ```

**[⬆ back to top](#table-of-contents)**

## Testing

  <a name="testing--yup"></a><a name="17.1"></a>
  - [17.1](#testing--yup) **Yup.**

  <a name="testing--for-real"></a><a name="17.2"></a>
  - [17.2](#testing--for-real) **No, but seriously**:
    - Whichever testing framework you use, you should be writing tests!
    - Strive to write many small pure functions, and minimize where mutations occur.
    - Write tests as you develop instead of piling them up for later. Fix problems early on, otherwise they will compound.
    - Be cautious about stubs and mocks - they can make your tests more brittle.
    - Avoid side effects to your development/production code/assets. Setup a separate database and set of assets for testing.
    - 100% test coverage is a good goal to strive for, even if it’s not always practical to reach it.
    - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

  <a name="testing--direct-assertation"></a><a name="17.3"></a>
  - [17.3](#testing--direct-assertation) Use direct assertations and explicit comparisons; avoid negations.

    > Why? Make the expected result for comparison explicit and avoid any implicit type conversion.

    ```python
    # bad - Other values can be falsy too: `[], 0, '', None`
    assert not result
    assert result_list

    # good
    assert result == False
    assert len(result_list) > 0
    ```

**[⬆ back to top](#table-of-contents)**

## Resources

**Learning Python**

  - [Learn Python in 10 minutes](https://www.stavros.io/tutorials/python/)

**Tools**

  - Code Style Linters
    - [Python PEP8 Autoformat](https://packagecontrol.io/packages/Python%20PEP8%20Autoformat)

**Other Style Guides**

  - [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
  - [Great example: spaCy source code](https://github.com/explosion/spaCy)

**[⬆ back to top](#table-of-contents)**

## Contributors

  - [View Contributors](https://github.com/kengz/python/graphs/contributors)

## Amendments

We encourage you to fork this guide and change the rules to fit your team’s style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.
