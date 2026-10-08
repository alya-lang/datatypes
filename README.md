# datatypes

[![CI](https://github.com/alya-lang/datatypes/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/datatypes/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/datatypes?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fdatatypes%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fdatatypes%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Advanced data structures for Alya: RingBuffer, DoublyLinkedList, Trie, BitSet, BloomFilter and more

---

## 🌟 Features

- ⚡ **Lightweight & High Performance**: Zero unnecessary heap overhead, fast native operations across all data structures
- 🔄 **RingBuffer**: Fixed-capacity circular FIFO buffer supporting O(1) push, pop, peek, and capacity overwriting
- 🔗 **DoublyLinkedList**: Doubly-linked list with O(1) push/pop at both front and back ends
- 🌳 **Trie (Prefix Tree)**: High-speed string indexing, prefix search (`starts_with`), and autocompletion (`keys_with_prefix`)
- 🔢 **BitSet**: Compact 64-bit word-aligned bit array with bitwise set algebra (`union`, `intersection`), popcount, and iteration
- 🌸 **BloomFilter**: Probabilistic set membership filter with configurable capacity and dual-hash distribution
- 🧪 **Comprehensive Verification**: 100% test coverage with automated unit tests and throughput micro-benchmarks

---

## 📁 Project Architecture

```
datatypes/
├── .alyalint               # Linter configuration (rules, exclusions, severity overrides)
├── .editorconfig           # Uniform formatting rules across IDEs and editors
├── .gitignore              # Ecosystem standard ignore filters
├── .vscode/                # VS Code workspace settings, DAP launch configurations & tasks
├── alya.toml               # Package manifest with dependencies and metadata
├── src/
│   ├── lib.alya            # Public API facade and constructor exports
│   ├── types.alya          # Core struct definitions (RingBuffer, Trie, BitSet, etc.)
│   ├── ring_buffer.alya    # Circular buffer implementation
│   ├── linked_list.alya    # Doubly-linked list implementation
│   ├── trie.alya           # Trie prefix tree implementation
│   ├── bit_set.alya        # Word-backed bitset implementation
│   └── bloom_filter.alya   # Bloom filter implementation
├── examples/
│   └── demo.alya           # Comprehensive runnable walkthrough of all data structures
├── tests/
│   └── test_basic.alya     # Automated test suite with 90+ test assertions
└── benches/
    └── bench_basic.alya    # Micro-benchmarks measuring throughput
```

> [!NOTE]
> **Visibility & Modularity:** Symbols annotated with `pub` (`pub function`, `pub struct`) are exported to external callers. Internal helper functions remain private to prevent namespace pollution.

---

## 📦 Installation

Add `datatypes` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
datatypes = { git = "https://github.com/alya-lang/datatypes", branch = "main" }
```

Or install it directly using the Alya package CLI:

```bash
alya add datatypes --git https://github.com/alya-lang/datatypes --branch main
alya install
```

---

## 🚀 Quick Start

```alya
import "datatypes" as pkg

function main()
    # 1. Circular RingBuffer
    let rb = pkg::new_ring_buffer(3)
    rb.push("alpha")
    rb.push("beta")
    say f"RingBuffer peek: {rb.peek()}"

    # 2. DoublyLinkedList
    let list = pkg::new_linked_list()
    list.push_back("Tail")
    list.push_front("Head")
    say f"List front: {list.peek_front()}"

    # 3. Trie Prefix Tree
    let tr = pkg::new_trie()
    tr.insert("apple", 1)
    say f"Has 'apple': {tr.contains('apple')}"
    say f"Matches 'app': {tr.keys_with_prefix('app')}"

    # 4. BitSet
    let bs = pkg::new_bit_set(64)
    bs.set(10)
    say f"Bit 10 is set: {bs.get(10)}"

    # 5. BloomFilter
    let bf = pkg::new_bloom_filter(100, 0.01)
    bf.add("test_key")
    say f"BloomFilter contains: {bf.contains('test_key')}"
end

main()
```

---

## 📖 API Reference

| Symbol | Visibility | Description |
|---|---|---|
| `new_ring_buffer(capacity)` | `pub function` | Constructs a new circular `RingBuffer` with the specified slot capacity. |
| `RingBuffer.push(item)` | `pub method` | Appends an item to the buffer. Returns false if full. |
| `RingBuffer.push_overwrite(item)` | `pub method` | Appends an item, overwriting the oldest entry if full. |
| `RingBuffer.pop()` | `pub method` | Removes and returns the oldest item (FIFO). |
| `RingBuffer.peek()` | `pub method` | Returns the oldest item without removing it. |
| `RingBuffer.peek_last()` | `pub method` | Returns the newest item without removing it. |
| `RingBuffer.is_full()` | `pub method` | Returns true if the buffer has reached capacity. |
| `RingBuffer.is_empty()` | `pub method` | Returns true if the buffer holds zero elements. |
| `RingBuffer.size()` | `pub method` | Returns the count of active buffered elements. |
| `RingBuffer.to_array()` | `pub method` | Returns elements as an array ordered from oldest to newest. |
| `new_linked_list()` | `pub function` | Constructs a new empty `DoublyLinkedList`. |
| `DoublyLinkedList.push_back(val)` | `pub method` | Appends a value to the tail of the list in O(1). |
| `DoublyLinkedList.push_front(val)` | `pub method` | Prepends a value to the head of the list in O(1). |
| `DoublyLinkedList.pop_back()` | `pub method` | Removes and returns the tail element in O(1). |
| `DoublyLinkedList.pop_front()` | `pub method` | Removes and returns the head element in O(1). |
| `DoublyLinkedList.peek_front()` | `pub method` | Returns the head element without removing it. |
| `DoublyLinkedList.peek_back()` | `pub method` | Returns the tail element without removing it. |
| `DoublyLinkedList.contains(val)` | `pub method` | Returns true if the list contains the value. |
| `DoublyLinkedList.remove(val)` | `pub method` | Removes the first occurrence of the value. |
| `DoublyLinkedList.to_array()` | `pub method` | Returns elements as an array from head to tail. |
| `new_trie()` | `pub function` | Constructs a new empty `Trie` prefix tree. |
| `Trie.insert(key, value)` | `pub method` | Inserts a string key with an associated payload into the Trie. |
| `Trie.contains(key)` | `pub method` | Returns true if the exact key exists in the Trie. |
| `Trie.get(key)` | `pub method` | Returns the value associated with the key, or null if missing. |
| `Trie.starts_with(prefix)` | `pub method` | Returns true if any key in the Trie starts with prefix. |
| `Trie.keys_with_prefix(prefix)` | `pub method` | Returns an array of all keys beginning with prefix. |
| `Trie.remove(key)` | `pub method` | Removes a key from the Trie. |
| `new_bit_set(num_bits)` | `pub function` | Constructs a new `BitSet` with capacity for at least `num_bits`. |
| `BitSet.set(bit)` | `pub method` | Sets the bit at index `bit` to 1. |
| `BitSet.clear_bit(bit)` | `pub method` | Clears the bit at index `bit` (sets to 0). |
| `BitSet.toggle(bit)` | `pub method` | Inverts the bit at index `bit`. |
| `BitSet.get(bit)` | `pub method` | Returns true if the bit at index `bit` is 1. |
| `BitSet.count_ones()` | `pub method` | Returns the total count of set bits (population count). |
| `BitSet.union(other)` | `pub method` | Computes the bitwise OR with another BitSet. |
| `BitSet.intersection(other)` | `pub method` | Computes the bitwise AND with another BitSet. |
| `BitSet.to_array()` | `pub method` | Returns an array of active bit indices. |
| `new_bloom_filter(expected, fp_rate)` | `pub function` | Constructs a new `BloomFilter` configured for expected elements and false positive rate. |
| `BloomFilter.add(item)` | `pub method` | Inserts an item into the BloomFilter. |
| `BloomFilter.contains(item)` | `pub method` | Probabilistic test: returns true if item is probably in set, false if definitely not. |
| `BloomFilter.count()` | `pub method` | Returns the count of added elements. |

---

## 🧪 Running Tests & Benchmarks

Run the automated test suite using `alya test`:

```bash
alya test
```

Generate static API documentation:

```bash
alya doc . -o docs --markdown
```

Run the benchmark suite:

```bash
alya run benches/bench_basic.alya
```

Run the example demo:

```bash
alya run examples/demo.alya
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

### 💻 Developer Tooling & VS Code Integration

This package comes preconfigured with recommended workspace settings and tasks for **Visual Studio Code**:
- **LSP & Formatting**: Auto-formatting on save and real-time Language Server diagnostics via `alya-lang.vscode-alya`.
- **DAP Debugging**: Launch configurations in `.vscode/launch.json` ready for interactive step-debugging via `F5`.
- **Predefined Tasks**: Press `Ctrl+Shift+B` or run tasks (`Test`, `Lint`, `Format`, `Build Docs`) directly from the Command Palette.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.