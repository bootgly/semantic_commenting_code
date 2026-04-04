<p align="center">
  <img src="https://github.com/bootgly/.github/raw/main/bootgly-logo.128x128.jpg" alt="scc-logo" width="120px" height="120px"/>
</p>
<h1 align="center">Semantic Commenting Code</h1>
<p align="center">
  <i>A standard for structuring code comments to improve AI autocomplete, code predictability, and readability.</i>
</p>
<p align="center">
  <a href="https://github.com/bootgly/semantic_commenting_code">
    <img alt="License" src="https://img.shields.io/github/license/bootgly/semantic_commenting_code"/>
  </a>
</p>

> Originated from the [Bootgly PHP Framework](https://github.com/bootgly/bootgly). Language-agnostic.

---

## What is Semantic Commenting Code?

**Semantic Commenting Code (SCC)** is a convention for inline comments that gives each comment a **semantic role** based on a single-character marker. Instead of writing free-form comments, you prefix each comment with a symbol that tells both humans and AI tools *what kind of operation follows*.

This is similar to how [Semantic Versioning](https://semver.org/) gives meaning to version numbers — SCC gives meaning to inline comments.

### Why?

- **AI autocomplete** — LLMs recognize the pattern and predict the next code section type accurately
- **Code predictability** — developers know exactly where guards, setup, logic, and returns live
- **Quick readability** — scan a method in seconds by reading only the markers
- **One-way structure** — every method follows the same flow, reducing decision fatigue
- **Language-agnostic** — works in any language that supports `//` or `#` comments

---

## The Standard

### Method Body Markers

Every method/function body follows this flow:

| Marker | Name | Purpose | When to use |
|--------|------|---------|-------------|
| `// ?` | **Guard** | Precondition, validation, early return | First thing in the method — check invariants |
| `// !` | **Init** | Initialization, setup, variable preparation | After guards — prepare what's needed |
| `// @` | **Execute** | Main logic, the action being performed | The core purpose of the method |
| `// :` | **Return** | Return statement | End of method (`// ?:` for conditional return) |
| `// ---` | **Separator** | Logical phase boundary | Between distinct phases within a section |

### Class Property Markers

Properties are grouped in a fixed order:

| Marker | Name | Purpose |
|--------|------|---------|
| `// * Config` | **Config** | Public, injectable settings |
| `// * Data` | **Data** | Public or protected, injectable data |
| `// * Metadata` | **Metadata** | Protected or private, not injectable |

### Structural Markers

| Marker | Name | Purpose |
|--------|------|---------|
| `// #` | **Subsection** | Named group within a section (e.g., `// # Socket`) |
| `// ---` | **Separator** | Visual/logical divider between phases |

---

## Example

### PHP

```php
public function connect (string $host, int $port): bool
{
   // ?
   if (empty($host))
      return false;

   // !
   $socket = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);

   // @
   $connected = socket_connect($socket, $host, $port);

   // :
   return $connected;
}
```

### Python

```python
def connect(self, host: str, port: int) -> bool:
   # ?
   if not host:
      return False

   # !
   self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

   # @
   self.socket.connect((host, port))

   # :
   return True
```

### TypeScript

```typescript
function connect (host: string, port: number): boolean {
   // ?
   if (!host) return false;

   // !
   const socket = new Socket();

   // @
   socket.connect(port, host);

   // :
   return true;
}
```

### Class Properties

```php
class Server
{
   // * Config
   public string $host = '0.0.0.0';
   public int $port = 8080;

   // * Data
   public protected(set) resource $socket;

   // * Metadata
   private bool $started = false;
   private int $connections = 0;
}
```

---

## Rules

1. **Markers are optional** — omit a section if the method doesn't need it (e.g., no `// ?` if there are no guards)
2. **Order is fixed** — `// ?` → `// !` → `// @` → `// :` (never rearrange)
3. **Markers stand alone** — place the marker on its own line, code follows on the next line(s)
4. **One flow per method** — if a method needs multiple `// @` sections, consider splitting into smaller methods
5. **Property groups are exhaustive** — every class property belongs to exactly one of the three groups

---

## Adoption

SCC is currently used throughout the [Bootgly PHP Framework](https://github.com/bootgly/bootgly) codebase (700+ files).

To adopt in your project:
1. Start using the markers in new code
2. Refactor existing methods incrementally
3. Add a reference to this spec in your contributing guidelines

---

## Specification Version

**SCC 1.0.0** (February 2026)

This specification follows [Semantic Versioning](https://semver.org/).

---

## License

[MIT](LICENSE)

---

<p align="center">
  Created by <a href="https://github.com/bootgly">Bootgly</a>
</p>
