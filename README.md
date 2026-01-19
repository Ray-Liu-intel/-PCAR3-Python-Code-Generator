# PCAR3 Python Code Generator 2.0

A powerful, browser-based web UI tool for generating PCAR3 Python code with advanced loop partition capabilities. This single-file HTML application enables users to build complex PlistFile structures with PLBs (Pattern List Blocks), pattern management, and dynamic loop generation without needing any server infrastructure.
## ☀️PCAR3 Differences between the ToS3 and ToS4 versions
Due to the change from tos3 to tos4, the syntax of our Plistfile also has undergone some modifications.Previously in TOS3 PLists the many options we had (PreBurst, PostBurst, PrePattern, etc.) were written in a more compressed view in bracket blocks after the PList name. Now, in TOS4 PDE's will be able to see a more explicit view of what content will execute in what order and any form of ambiguity should not be an issue. Because this is more flattened than previously done users may need to develop or adapt to the new changes as not all previous functions in pcar will naturally convert to a TOS4 compatible plist.The tos4 plist structure was designed as a "what you see is what you get" and we must follow the syntax order of the following images.Using the layout as above will indeed make the pcar/plists longer but it will be created exactly as it is written without compressing the view/functionality. 
<img width="1099" height="419" alt="image" src="https://github.com/user-attachments/assets/ff50c801-8975-4b9d-a4a2-f58ecba5f288" />

### PList Flatternd View
| Attribute | Description |
|---------|---------------|
| Version | Plist file should start with  should exist on the beginning of the file (was Version 5.0 for TOS3*). <br>File can be empty as long as it has the version statement.Version 6.0 |
| PList | The  keyword specifies the beginning of the Pattern List Block.<br>Pattern Lists can include other Pattern Lists in a nested structure using the  keyword. However, Pattern Lists at a lower level cannot include a Pattern List from a higher level, as that would constitute an infinite loop on Pattern Lists (recursive). Such scenarios are checked at load time and will result in a load time failure.PListRefPList` |
| PreExecRefPList | The  keyword is used to reference a previously defined RefPList which is to be executed in the beginning of the PList. If used, this **element must precede** any Pat or RefPList elements. Can be used along with PreExecPat elements and multiple PreExecRefPList elements may be used.PreExecRefPList |
| PreExecPat | The  keyword is used to reference a previously defined Pat which is to be executed in the beginning of the PList. If used, this **element must precede** any Pat or RefPList elements. Can be used along with PreExecRefPList elements and multiple PreExecPat elements may be used.PreExecPat |
| Pat | The  keyword specifies a Pattern to be executed. A user can specify as many Pats as needed.Pat<br>In this project, we can use the string or list of **Tuples** and **Tids** to directly search for patterns in Trace Central, or indirectly search by test name (also known as pattern name) using **Querytid**.|
| RefPList | The  keyword specifies a referential link to another existing PList defined previously in the current .plist file, or a previously defined .plist file. A user can specify as many RefPLists as needed, within a PList.RefPList |
| PostExecRefPList | The  keyword indicates a previously defined RefPList which is to be executed prior to the end of the current PList.<br>In the case of all passing elements, this is obvious. But, in the case where the PList is executed using a "non-stop" capture mode and a failing element occurs, the PListName referenced by the PostExecRefPList will be executed after the failing element, prior to the PList finishing its execution.<br>**No Pat or RefPList elements are allowed after** the PostExecRefPList/PostExecPat elements. Can be used along with the PostExecPat elements and multiple PostExecRefPList elements may be used.PostExecRefPList |
| PostExecPat | The  keyword indicates a pattern which is to be executed prior to the end of the current PList.<br>In the case of all passing elements, this is obvious. But, in the case where the PList is executed using a "non-stop" capture mode and a failing element occurs, the Pattern referenced by the PostExecPat will be executed after the failing element, prior to the PList finishing its execution.<br>**No Pat or RefPList elements are allowed after** the PostExecPat elements. Can be used along with the PostExecRefPList elements and multiple PostExecPat elements may be used.PostExecPat |

### Parameter Variation
| TOS3 | TOS4 |
|---------|---------------|
| GlobalPlist​ | Plist​ |
| Plist​ | RefPlist​ |
| PreBurst/PreBurstPlist​ | PreExecPat/PreExecRefPList​ |
| PostBurst/PostburstPlist​ | PostExecPat/PostExecRefPList ​ |
| PrePattern/PostPattern​​ | PrePattern/PostPattern​​(add for each main pattern) |
| PrePList/PostPList​ | PrePList/PostPList(add for each main pattern)​ |
| Mask -> Plist | DefaultPListPinMask ​ |
| Mask -> Pattern | PatPinMask​ |
| <img width="1106" height="820" alt="image" src="https://github.com/user-attachments/assets/ddbe2b78-31f7-43da-b492-447506262e4f" /> | <img width="774" height="459" alt="image" src="https://github.com/user-attachments/assets/90151416-bef2-4d29-b807-2c9b5975e5e1" /> |

### Important Links
[DMR Tos4 PCAR3 notes for PDE's](https://wiki.ith.intel.com/spaces/TVPV/pages/3785317182/DMR+Tos4+PCAR3+notes+for+PDE+s#DMRTos4PCAR3notesforPDE's-TOS4PList_BuilderCommandLineRequirements)<br>
[Plist syntax differences for TOS3 and TOS4](https://wiki.ith.intel.com/spaces/HTD/pages/4045277291/Plist+syntax+differences+for+TOS3+and+TOS4)<br>
[PCAR3 full reference](https://wiki.ith.intel.com/spaces/TVPV/pages/1207039914/PCAR3+full+reference#PCAR3fullreference-Tids())<br>
[PCAR3 Examples](https://wiki.ith.intel.com/spaces/TVPV/pages/1404112654/PCAR3+Examples)Only focus on Tos4 at bottom of the link


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

### Generate Pcar3 In Web

#### Basic Usage

1. **Open the File**:
   - Double-click `Core Code2.0.HTML` to open in your default browser
   - Or drag the file into an open browser window

2. **Configure Your PlistFile**:
![Screenshot_15-1-2026_145932_](https://github.com/user-attachments/assets/0564b5d1-4fe9-46ff-85b7-b8859ed386f4)
   ```
   - Enter PlistFile name (e.g., "sample2.plist")
   - Click "Add Plb" to create a new Pattern List Block
   ```

4. **Configure PLB**:
<img width="1039" height="341" alt="image" src="https://github.com/user-attachments/assets/076130bb-2abf-4053-b42b-1e01b85729d7" />
<img width="1551" height="953" alt="image" src="https://github.com/user-attachments/assets/c15c79f1-2944-4d8f-9a5a-d2beb8caaec2" />

   ```
   - Enter PLB Name
   - (Optional) Enter PLB Header text which is the PLB annotation before code
   - Configure prepattern/postpattern using mode selectors （!Pay attention：Only one of the four 'preplist' 'prepattern' 'postplist' 'postpattern' can be used!）
   - Add items using the "Add Pats", "Add RefPlb", "Add Comment" buttons
   --For detailed parameter rules, refer to the  Rules on Structures and Usage table
   ```


6. **Generate Code**:
   ```
   - Click "Generate Code" button
   - Review the generated Python code in the text area
   - Click "Copy Code" to copy to clipboard
   ```

#### Advanced Using Loop Partition

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
   
### Transfer pcar3 code to Flash
1. **Submi PCAR3 Config file**:
   ```
   - Left bar > Job Submission > Trace Conversion
   - Create a. pcar file in UNIX and copy the generated code into it
   - Select PCAR Config and fill in the UNIX path of the PCAR3 config file
   - Tick on Plist Builder refresh
   - Fill in the rest of the form based on your test requirement (similar to trace conversion WebUI form)
   ```
   <img width="1327" height="727" alt="image" src="https://github.com/user-attachments/assets/f2ef0d78-50cd-4695-8675-dd64bf1e4c69" />
   <img width="550" height="547" alt="image" src="https://github.com/user-attachments/assets/368462b1-147f-4c1c-8cbd-20b6ca9cedd3" />

   
3. **Check the PList result**:
   ```
   - Left bar > Job Submission > Results
   - Plist file > Copy Path to Clipboard (Directory of Plist Builder Results) and gvim in the UNIX.
   ```
   <img width="2121" height="132" alt="image" src="https://github.com/user-attachments/assets/a7b27f60-28d9-464b-b10d-9e9cece68572" />

## 💡 Usage Examples

### Example 1: Simple PLB without Loop



```html
<!-- Configuration -->
PlistFile Name: test.plist
PLB Name: my_plb
prepattern: Tuple mode → ['pre_pattern']
postpattern: None

Add Pats (Tuple mode): 0000123,0000456
Add Comment: This is a test
```

**Generated Code:**
```python
from vgplib.plist_builder_pcar3 import PlistFile, Plb, Pats, Tids, QueryTid, pcar3, Amble, Comment, RefPlb, NameFilter, PreExecPat, PostExecPat
pcar3.tos_ver = 'tos4'

level0 = PlistFile('test.plist')

level1 = level0.add(Plb("my_plb", prepattern=Pats(['pre_pattern'])))
level1.add(Pats(['0000123','0000456']))
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
