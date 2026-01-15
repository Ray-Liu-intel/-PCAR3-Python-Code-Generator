![Screenshot_15-1-2026_145932_](https://github.com/user-attachments/assets/ffa16fa3-6fb8-4a84-8289-9d999df930c7)# PCAR3 Python Code Generator 2.0

A powerful, browser-based web UI tool for generating PCAR3 Python code with advanced loop partition capabilities. This single-file HTML application enables users to build complex PlistFile structures with PLBs (Pattern List Blocks), pattern management, and dynamic loop generation without needing any server infrastructure.

## 🎯 Features

### Core Functionality
- **Single-File Application**: Complete functionality in one HTML file - no server required
- **Drag & Drop Sorting**: Reorder PLB items and loop items using intuitive drag handles
- **Real-Time Code Generation**: Instantly generate Python code from your configuration
- **Multiple PLBs**: Create and manage multiple Pattern List Blocks with collapsible sections
- **Copy to Clipboard**: One-click code copying for easy integration

### Pattern Management
- **Multiple Pattern Modes**:
  - **Tuple Mode**: Simple pattern lists like `Pats(['0000123', '0000456'])`
  - **Tids Mode**: Test ID patterns with optional chunk parameter
  - **QueryTid Mode**: Advanced query-based patterns with testname, testtype, and chunk
- **Pre/Post Patterns**: Configure prepattern and postpattern for each PLB
- **Pre/Post Execution Patterns**: Add PreExecPat and PostExecPat with full mode support
- **Reference PLists**: Support for PreExecRefPList, PostExecRefPList, and RefPlb

### Loop Partition System
- **Global Loop Configuration**:
  - Toggle to enable/disable loop partition mode
  - Multi-line textarea for `name_of_instance` entries
  - Auto-sync feature: names automatically populate the decode_par mapping table
  - Dynamic decode_par mapping table (name → partition string)

- **PLB Header Support**:
  - Customizable header text for each PLB
  - Generates decorative comment blocks:
    ```python
    level1 = level0.add(    Comment(' '))
    level1 = level0.add(    Comment('---------------------------'))
    level1 = level0.add(    Comment('        YOUR HEADER        '))
    level1 = level0.add(    Comment('---------------------------'))
    level1 = level0.add(    Comment(' '))
    ```

- **Unified Loop Structure**:
  - Single `for name in name_of_instance:` loop per PLB
  - Visual blue dashed border clearly demarcates loop section
  - Main loop items (executed directly in the loop)
  - Conditional blocks (if/elif/else) within the same loop
  - Sortable items with drag and drop reordering

- **Intelligent level1_sub Generation**:
  - When RefPlb items exist in main loop, automatically generates:
    ```python
    level1_sub = level0.add(Plb('corresponding_plb_name'.format(name=name)))
    ```
  - Items in condition blocks use `level1_sub.add(...)`
  - Main loop items use `level1.add(...)`

- **Placeholder Support**:
  - Use `{name}` and `{partition}` in any string field
  - Automatically replaced with `.format(name=name, partition=partition)`
  - Works in PLB names, pattern values, comments, and all text fields

### User Interface Enhancements
- **Simplified Main Loop Buttons**: Only essential buttons (Add Pats, Add RefPlb, Add Comment) visible in main loop section
- **Full Feature Set in Conditions**: All button types available in if/elif/else blocks
- **Visual Clarity**: Blue dashed border groups loop items and condition blocks together
- **Responsive Design**: Clean, modern interface with Intel branding
- **Collapsible PLBs**: Minimize sections you're not actively editing

## 📋 Requirements

- Modern web browser (Chrome, Firefox, Edge, Safari)
- No server or installation required
- No external dependencies (self-contained HTML file)

## 🚀 Quick Start

### Basic Usage

1. **Open the File**:
   - Double-click `Core Code2.0.HTML` to open in your default browser
   - Or drag the file into an open browser window

2. **Configure Your PlistFile**:
   ```
   - Enter PlistFile name (e.g., "sample2.plist")
   - Click "Add Plb" to create a new Pattern List Block
   ```

3. **Configure PLB**:
   ```
   - Enter PLB Name
   - (Optional) Enter PLB Header text
   - Configure prepattern/postpattern using mode selectors
   - Add items using the "Add Pats", "Add RefPlb", "Add Comment" buttons
   ```

4. **Generate Code**:
   ```
   - Click "Generate Code" button
   - Review the generated Python code in the text area
   - Click "Copy Code" to copy to clipboard
   ```

### Using Loop Partition

1. **Enable Loop Partition**:
   ```
   ☑ Check "Enable Loop Partition" checkbox
   ```

2. **Configure name_of_instance**:
   ```
   - Enter instance names in the textarea (one per line)
   - Example:
     extemca
     intemca27A
     intemca27B
   ```

3. **Configure decode_par Mapping**:
   ```
   - Names from name_of_instance automatically appear in the table
   - Fill in the "Partition String" column for each name
   - Example:
     intemca27A → P27A
     intemca27B → P27B
   ```

4. **Add PLB with Loop Support**:
   ```
   - Click "Add Plb"
   - Enter PLB Name and optional Header
   - Configure prepattern/postpattern (no loop here)
   - Scroll to "Loop Items (for name in name_of_instance)" section
   ```

5. **Add Main Loop Items**:
   ```
   - Click "Add Pats" or "Add RefPlb" or "Add Comment"
   - Use {name} and {partition} placeholders in values
   - Example RefPlb: scn_class_asic_{name}_edt_chain_iscan_end_list
   - Drag items to reorder them
   ```

6. **Add Conditional Logic**:
   ```
   - Click "Add if", "Add elif", or "Add else" buttons
   - Enter condition (e.g., name == 'intemca27A' or name == 'intemca27B')
   - Add items to the condition block using all available buttons
   - All items in condition blocks automatically use level1_sub
   ```

7. **Generate Loop Code**:
   ```
   - Click "Generate Code"
   - Code includes:
     * name_of_instance array
     * decode_par() function
     * PLB header comments (if specified)
     * for loop with level1.add(...) for main items
     * if/elif/else blocks with level1_sub.add(...) for condition items
     * Proper indentation and .format() calls
   ```

## 💡 Usage Examples

### Example 1: Simple PLB without Loop

![](PCAR3-Python-Code-Generator/Image/Screenshot_15-1-2026_145932_.jpeg)


```html
<!-- Configuration -->
PlistFile Name: test.plist
PLB Name: my_plb
prepattern: Tuple mode → ['pre_pattern']
postpattern: None

Add Pats (Tuple mode): ['0000123', '0000456']
Add Comment: This is a test
```

**Generated Code:**
```python
from vgplib.plist_builder_pcar3 import PlistFile, Plb, Pats, Tids, QueryTid, pcar3, Amble, Comment, RefPlb, NameFilter, PreExecPat, PostExecPat
pcar3.tos_ver = 'tos4'

level0 = PlistFile('test.plist')

level1 = level0.add(Plb("my_plb", prepattern=Pats(['pre_pattern'])))
level1.add(Pats(['0000123', '0000456']))
level1.add(Comment('This is a test'))

if (__name__ == "__main__"):
    pcar3.process()
    print("Summary")
    pcar3.report(detail=True)
    print(pcar3.gen_plist_preview())
```

### Example 2: PLB with Loop Partition

```html
<!-- Global Configuration -->
☑ Enable Loop Partition
name_of_instance:
  intemca27A
  intemca27B

decode_par Mapping:
  intemca27A → P27A
  intemca27B → P27B

<!-- PLB Configuration -->
PLB Name: scn_class_asic_intest_emca_edt_chain_iscan_end_list
PLB Header: CHAIN PLIST

Loop Items:
  - RefPlb: scn_class_asic_{name}_edt_chain_iscan_end_list

Condition if: name == 'intemca27A' or name == 'intemca27B'
  - Pats (QueryTid): testname='EMCA_{partition}_edt_chain_smux', testtype='scan_midamble', chunk="1"
  - Comment: abcd

Condition else:
  - Pats (Tuple): ['0000086']
  - Comment: 1234
```

**Generated Code:**
```python
from vgplib.plist_builder_pcar3 import PlistFile, Plb, Pats, Tids, QueryTid, pcar3, Amble, Comment, RefPlb, NameFilter, PreExecPat, PostExecPat
pcar3.tos_ver = 'tos4'

name_of_instance = ['intemca27A', 'intemca27B']

def decode_par(name):
    if name == 'intemca27A':
        return 'P27A'
    elif name == 'intemca27B':
        return 'P27B'
    else:
        return 'NOMATCH'

level0 = PlistFile('sample2.plist')

level1 = level0.add(    Comment(' '))
level1 = level0.add(    Comment('---------------------------'))
level1 = level0.add(    Comment('        CHAIN PLIST        '))
level1 = level0.add(    Comment('---------------------------'))
level1 = level0.add(    Comment(' '))
level1 = level0.add(Plb("scn_class_asic_intest_emca_edt_chain_iscan_end_list"))

for name in name_of_instance:
    partition = decode_par(name)
    level1.add(RefPlb('scn_class_asic_{name}_edt_chain_iscan_end_list'.format(name=name)))
    if (name == 'intemca27A' or name == 'intemca27B'):
        level1_sub = level0.add(Plb('scn_class_asic_{name}_edt_chain_iscan_end_list'.format(name=name)))
        level1_sub.add(Pats(QueryTid(testname='EMCA_{partition}_edt_chain_smux'.format(partition=partition), testtype='scan_midamble', chunk="1")))
        level1_sub.add(Comment('abcd'))
    else:
        level1_sub = level0.add(Plb('scn_class_asic_{name}_edt_chain_iscan_end_list'.format(name=name)))
        level1_sub.add(Pats(['0000086']))
        level1_sub.add(Comment('1234'))

if (__name__ == "__main__"):
    pcar3.process()
    print("Summary")
    pcar3.report(detail=True)
    print(pcar3.gen_plist_preview())
```

### Example 3: Advanced Pattern Modes

```html
<!-- PLB with multiple pattern types -->
PLB Name: advanced_plb

Loop Items:
  - Pats (QueryTid): testname='test.*', testtype='scan', chunk="2"
  - PreExecPat (Tids mode): ['TID123', 'TID456'], chunk="1"
  - PostExecRefPList: post_execution_list
  - RefPlb: reference_plb_{name}
  - Comment: Advanced pattern configuration

if: partition == 'P27A'
  - PreExecRefPList: pre_list_{partition}
  - Pats (Tuple): ['pattern_{name}']
  - PostExecPat (QueryTid): testname='post_{name}', testtype='functional', chunk="3"
```

## 🎨 UI Components

### Button Layout

**Main Loop Items Section** (simplified):
- ✅ Add Pats
- ✅ Add RefPlb
- ✅ Add Comment
- ❌ PreExec/PostExec buttons (hidden for simplicity)

**Condition Blocks** (full feature set):
- ✅ Add Pats
- ✅ Add PreExecRefPList
- ✅ Add PreExecPat
- ✅ Add RefPlb
- ✅ Add PostExecRefPList
- ✅ Add PostExecPat
- ✅ Add Comment

### Visual Indicators

- **Blue Dashed Border**: Clearly marks loop section boundaries
- **Drag Handles (☰)**: Indicates draggable items
- **Collapse Button (∨/∧)**: Toggle PLB visibility
- **Delete Buttons**: Remove items or PLBs

## 🔧 Technical Details

### Pattern Mode Mapping

| UI Mode | Generated Code |
|---------|---------------|
| Tuple | `Pats(['value1', 'value2'])` |
| Tids | `Pats(Tids(['TID1', 'TID2']), chunk="...")` |
| QueryTid | `Pats(QueryTid(testname='...', testtype='...'), chunk="...")` |

### Loop Item Types

| Type | Main Loop Variable | Condition Variable |
|------|-------------------|-------------------|
| Pats | `level1.add(Pats(...))` | `level1_sub.add(Pats(...))` |
| RefPlb | `level1.add(RefPlb(...))` | Auto-generates `level1_sub = level0.add(Plb(...))` |
| Comment | `level1.add(Comment(...))` | `level1_sub.add(Comment(...))` |
| PreExecPat | N/A (hidden) | `level1_sub.add(PreExecPat(...))` |
| PostExecPat | N/A (hidden) | `level1_sub.add(PostExecPat(...))` |
| PreExecRefPList | N/A (hidden) | `level1_sub.add(PreExecRefPList(...))` |
| PostExecRefPList | N/A (hidden) | `level1_sub.add(PostExecRefPList(...))` |

### Code Generation Logic

1. **Header**: Import statements and `pcar3.tos_ver` setting
2. **Loop Config** (if enabled):
   - `name_of_instance` array
   - `decode_par(name)` function with if/elif/else
3. **PlistFile**: `level0 = PlistFile('...')`
4. **PLB Header** (if specified): Decorative comment block
5. **PLB Definition**: `level1 = level0.add(Plb(...))`
6. **Loop** (if enabled):
   - `for name in name_of_instance:`
   - `partition = decode_par(name)`
   - Main loop items
   - if/elif/else blocks with auto-generated `level1_sub`
7. **Footer**: Standard execution block

## 📝 Tips and Best Practices

### Naming Conventions
- Use descriptive PLB names that indicate their purpose
- Include `{name}` and `{partition}` in loop-based names for clarity
- Use consistent naming patterns across related PLBs

### Loop Configuration
- Keep name_of_instance entries short and meaningful
- Use clear partition strings that match your test setup
- Add PLB headers to major sections for better code readability

### Pattern Management
- Start with Tuple mode for simple pattern lists
- Use QueryTid for dynamic test selection
- Add chunk parameters when working with partitioned patterns
- Leverage placeholders for dynamic values in loops

### Code Organization
- Collapse PLBs you're not actively editing
- Use drag handles to reorder items logically
- Group related conditions together
- Add comments to complex condition blocks

### Workflow Optimization
- Configure global loop settings before adding PLBs
- Use copy-paste for similar PLB configurations
- Test generated code incrementally
- Keep a backup of complex configurations

## 🐛 Troubleshooting

### Issue: Loop items not generating code
**Solution**: Ensure "Enable Loop Partition" is checked and name_of_instance is filled

### Issue: Placeholders not replaced
**Solution**: Use exact syntax `{name}` and `{partition}` (case-sensitive, with braces)

### Issue: level1_sub not generated
**Solution**: Add at least one RefPlb in main loop items, then add items to condition blocks

### Issue: Sortable drag not working
**Solution**: Ensure Sortable.js CDN is accessible (check browser console for errors)

### Issue: Pattern mode not showing correct fields
**Solution**: Select the mode from dropdown first, then fill in the values

## 🔄 Version History

### Core Code2.0.HTML (Latest)
- ✅ Unified loop structure with single for loop
- ✅ Blue dashed border for visual loop grouping
- ✅ PLB Header support with decorative comments
- ✅ Auto-sync name_of_instance to decode_par table
- ✅ Simplified button layout in main loop
- ✅ Full feature set in condition blocks
- ✅ Sortable items with drag and drop
- ✅ Auto-generation of level1_sub for RefPlb
- ✅ Chunk support for QueryTid and Tids in loop items
- ✅ {name} and {partition} placeholder support
- ✅ Hide subPlb button (not needed)
- ✅ Hide PreExec/PostExec in main loop
- ✅ Copyright updated to 2026

### Core Code1.0.HTML
- Basic PLB management
- Simple pattern configuration
- No loop partition support

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

This is a single-file HTML application. To contribute:
1. Make changes to `Core Code2.0.HTML`
2. Test thoroughly in multiple browsers
3. Ensure backward compatibility
4. Submit pull request with clear description

## 📧 Support

For issues, questions, or feature requests, please open an issue on the GitHub repository.

---

© 2026 INTEL PDG CHINA. All rights reserved.
