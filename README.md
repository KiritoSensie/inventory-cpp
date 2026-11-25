# inventory-cpp

A small inventory management app in C++17. It has no outside libraries, so it builds on a clean machine with only a
compiler and CMake.

## Layout

| Part | Files | Job |
| --- | --- | --- |
| Models | `src/models.*` | `Product`, `Order`, `Result` |
| Repositories | `src/repository.*` | Products and orders, kept in memory |
| Service | `src/inventory_service.*` | All the business rules |
| Controllers | `src/product_controller.*`, `src/stock_controller.*`, `src/order_controller.*` | Turn a command into a service call |
| Router | `src/router.*` | Sends a command line to the right controller |
| App | `src/main.cpp` | Reads commands from the terminal |
| Tests | `tests/test_inventory.cpp` | Run with `ctest` |

## Build and test

```bash
cmake -S . -B build
cmake --build build
ctest --test-dir build --output-on-failure
./build/inventory_app
```

## Commands 

```
product add <sku> <name> <quantity> <reorder level> <price cents>
product list
product show <sku>
product price <sku> <price cents>
product discount <sku> <percent>          (1 to 90 percent off)
product remove <sku>                      (refused while an open order uses it)
product export <file.csv>
product import <file.csv>
stock receive <sku> <quantity>
stock adjust <sku> <change> "<reason>"    (the change may be negative; stock never goes below zero)
stock low
stock reorder
stock value
order create <sku:quantity> [<sku:quantity> ...]
order fulfill <id>
order cancel <id>
order list
help | quit
```

Put a name with spaces in quotes, for example `product add A-1 "Blue Widget" 10 3 1999`. Prices are whole cents.

CSV files for `export` and `import` must be plain relative names ending in `.csv` (no absolute paths, no `..`). The first
line is `sku,name,quantity,reorder_level,price_cents`.
