# HUSH, NEXT PROMPT: Epigenetics, The Nuclear RNA Exosome, & Human Disease

Interactive bibliography network for the review manuscript by Andrew G. Newman & Prim B. Singh.

**[View the bibliography network →](https://qoldt.github.io/Exosome/bibliography/)**

---

## Bibliography Network

The interactive visualization displays 193 papers from the review's reference library as a force-directed graph. It is built from a Zotero RDF export and rendered with [D3.js](https://d3js.org/).

### How nodes are created

Each node represents one paper extracted from the Zotero RDF file (`EXOSOME.rdf`). Node metadata includes:
- **Label:** First author and year
- **Title:** Full paper title
- **Authors:** First three authors
- **Year:** Publication year
- **Tags:** Zotero tags assigned to the paper
- **Categories:** One or more thematic categories (see below)

**Node size** scales with the number of connections (degree).

**Node color** reflects the paper's primary category.

### How edges are created

Edges (connections between papers) are generated through two complementary methods:

#### 1. Tag-based edges (weight ≥ 2)
Papers sharing two or more Zotero tags are connected. These edges appear as **brighter, thicker lines** and represent the strongest bibliographic relationships. Example: two papers both tagged `exosome` and `heterochromatin` would be connected with weight 2.

#### 2. Category-based edges (weight = 1)
To connect papers lacking Zotero tags (71 of 193 papers had no tags), additional edges are generated based on category assignments:

- **Same category:** Papers assigned to the same thematic category are connected to 3 temporally proximal neighbors (within ±4 publication years). These edges appear as **fainter, thinner lines**.
- **Same affinity group:** Papers in related categories (e.g., NEXT Complex and PAXT Complex are both in the "exosome group") are connected to 2 cross-category neighbors.

Affinity groups:
| Group | Categories |
|-------|-----------|
| Exosome | Exosome Core, NEXT Complex, PAXT Complex, TRAMP Complex, RNA Surveillance, Structural Biology |
| Silencing | HP1/Heterochromatin, HUSH Complex, Chromatin/Epigenetics |
| TE/Immunity | Transposable Elements, Innate Immunity, Senescence/Aging |
| Disease | Disease, Disease/Neurodegeneration, Disease/Fibrosis, Cancer |
| Genome | 3D Genome, Protocadherin, Enhancer/Promoter RNA |

#### 3. Manual edges
A small number of edges were added manually to connect papers with known biological relationships not captured by tags or categories (e.g., Newman et al., 2025 to Souaifan et al., 2025).

### How papers are classified

Each paper is assigned a **primary category** (determining node color) and optionally **additional categories** (used for filtering).

Classification uses a two-pass approach:

#### Pass 1: Zotero tags
Papers with existing Zotero tags are matched to categories via a tag-to-category mapping (e.g., `exosome` → Exosome Core, `hp1` → HP1/Heterochromatin).

#### Pass 2: Title keyword matching
Papers not categorized by tags (or categorized as "Other") are classified by regex matching against their titles. Rules are evaluated in priority order — the first match determines the primary category. All matches populate the `all_categories` field.

| Priority | Pattern | Category |
|----------|---------|----------|
| 1 | protocadherin, Pcdh | Protocadherin |
| 2 | exosome, EXOSC, Rrp4x, Dis3, Mtr4 | Exosome Core |
| 3 | NEXT, ZCCHC8, RBM7 | NEXT Complex |
| 4 | PAXT, ZFC3H1, PABPN1 | PAXT Complex |
| 5 | TRAMP, Cid14, PAPD | TRAMP Complex |
| 6 | HP1, Swi6, CBX, chromodomain | HP1/Heterochromatin |
| 7 | HUSH, TASOR, MPP8, Periphilin, MORC2, SETDB1 | HUSH Complex |
| 8 | transposable, LINE-1, retrotransposon, ERV | Transposable Elements |
| 9 | cancer, tumor, myeloma, leukemia, melanoma | Cancer |
| 10 | senescence, aging, lamin B, SIRT6, telomere | Senescence/Aging |
| 11 | CTCF, TAD, cohesin, chromatin loop, Hi-C | 3D Genome |
| 12 | pontocerebellar, PCH, neurodegeneration, ALS, Alzheimer | Disease/Neurodegeneration |
| ... | *(additional patterns for RNA Processing, Enhancer/Promoter RNA, Chromatin/Epigenetics, Innate Immunity, Disease/Fibrosis, Structural Biology)* | |

Papers matching multiple patterns receive multiple categories. The **primary category** determines node color; **all categories** are used for the "Include related" filter mode.

A final manual curation pass corrected misclassifications (e.g., protocadherin papers initially categorized as "3D Genome" due to CTCF/cohesin keywords matching before protocadherin keywords) and eliminated the "Other" and "Genome Integrity" categories.

### Interactive features

- **Hover** any node to see its title, authors, categories, and highlight its connections
- **Drag** nodes to rearrange the layout
- **Scroll** to zoom in/out
- **Legend click** toggles category visibility
- **"only"** button (appears on legend hover) solos a single category
- **Filter modes:**
  - *Include related* — shows papers matching ANY of their categories
  - *Strict category* — shows only papers whose PRIMARY category is selected
  - *Show all* — resets filters
- **Min edge weight** slider filters by connection strength (1 = all edges, 2+ = tag-based only)
- **Label size** slider adjusts text visibility
- **Search** box filters by author, title, tag, or category name

### Data pipeline

```
Zotero library
    ↓ export
EXOSOME.rdf
    ↓ Python extraction (author, title, year, tags)
bibliography_network.json (nodes + edges)
    ↓ D3.js rendering
bibliography_network.html (interactive visualization)
```

### Files

```
Exosome/
├── README.md
├── bibliography/
│   ├── index.html          # Interactive network visualization
│   └── d3.v7.min.js        # D3.js library (local copy)
```
