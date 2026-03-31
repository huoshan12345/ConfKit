# ConfKit [![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE.TXT) [![Build](https://github.com/huoshan12345/ConfKit/actions/workflows/build.yml/badge.svg)](https://github.com/huoshan12345/ConfKit/actions/workflows/build.yml)

ConfKit is a lightweight and extensible .NET library for parsing and generating multiple configuration formats, including **INI** and **UCI (OpenWrt Unified Configuration Interface)**.

It provides a consistent programming model, supports structured configuration access, and enables seamless conversion to JSON for strong typing and serialization.

---

### 🧩 Multi-format Support
- INI configuration parsing and generation
- UCI (OpenWrt) configuration parsing and generation

### 🏗 Structured Configuration Model
- Hierarchical sections and entries
- Strongly-typed object model
- Preserve structure and ordering

### 🔄 JSON Interoperability
- Convert configurations to `JsonObject`
- Deserialize into custom strongly-typed classes
- Flexible integration with `System.Text.Json`

### ⚡ Developer Friendly
- Simple and intuitive API
- Works with `netstandard2.0` and modern .NET versions
- Easy to extend for additional formats (TOML, YAML, etc.)

||TargetFramework(s)|Package|
|----|----|----|
|ConfKit|![netstandard2.0](https://img.shields.io/badge/netstandard-2.0-30a14e.svg) ![net8.0](https://img.shields.io/badge/net-8.0-30a14e.svg) ![net9.0](https://img.shields.io/badge/net-9.0-30a14e.svg) ![net10.0](https://img.shields.io/badge/net-10.0-30a14e.svg) |[![](https://img.shields.io/nuget/v/ConfKit?logo=nuget&label=nuget)](https://www.nuget.org/packages/ConfKit)|

---

## Installation

```bash
dotnet add package ConfKit
```

## 🚀 Usage

### INI Support

#### Parse INI Content

```csharp
string iniContent = @"
[server.database]
host = localhost
port = 1433";

var config = IniParser.Parse(iniContent);

string host = config.Sections
    .First(s => s.Name == "server")
    .SubSections.First(s => s.Name == "database")
    .Entries.First(e => e.Key == "host").Value;
```

---

#### Create INI Configuration

```csharp
var config = new IniConfig
{
    Entries =
    [
        new IniEntry("debug", "true"),
    ],
    Sections =
    [
        new IniSection("server")
        {
            SubSections =
            [
                new IniSection("database")
                {
                    Entries =
                    [
                        new IniEntry("host", "localhost"),
                        new IniEntry("port", "1433")
                    ],
                },
            ],
        },
    ],
};

string output = config.ToString();
```

---

### UCI Support

#### Parse UCI Configuration

```csharp
string configContent = @"
package network

config interface 'lan'
    option type 'bridge'
    option ifname 'eth0'
    option proto 'static'
";

var config = UciParser.Parse(configContent);

Console.WriteLine(config.PackageName);
```

---

#### Create UCI Configuration

```csharp
var config = new UciConfig
{
    PackageName = "network",
    Sections =
    [
        new UciSection("interface", "lan")
        {
            Options =
            [
                new UciOption("type", "bridge"),
                new UciOption("ifname", "eth0"),
                new UciOption("proto", "static")
            ]
        }
    ]
};

string output = config.ToString();
```

---

### 🔄 JSON Conversion & Strong Typing

Both INI and UCI configurations can be converted into a `JsonObject`, making it easy to deserialize into custom types.

#### Example

```csharp
var config = IniParser.Parse(iniContent);

// Convert to JSON object
JsonObject json = config.ToSerializableJsonObject();

// Deserialize to custom type
var options = new JsonSerializerOptions
{
    NumberHandling = JsonNumberHandling.AllowReadingFromString
};

var result = json.Deserialize<MyConfig>(options);
```

---

## TODO

- [ ] Add support for reading and writing comments (current parser just ignores comments)

## License

MIT License

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.