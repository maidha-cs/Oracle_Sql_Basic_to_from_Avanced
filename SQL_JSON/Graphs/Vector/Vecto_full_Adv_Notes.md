# Oracle AI Database 26ai — AI Vector Search

## 1. Vector

A vector is a sequence of numerical values used to represent the features or semantic meaning of data.

Example:

```text
[0.21, -0.45, 0.78, 0.13]
```

Vectors can represent data such as text, images, audio, and other information in numerical form.

---

## 2. Generate Vector / Embedding

An embedding model converts original data into a numerical vector.

```text
Original Data
      ↓
Embedding Model
      ↓
Vector
```

Example:

```text
"Customer cannot login"
          ↓
   Embedding Model
          ↓
[0.12, -0.45, 0.78, ...]
```

The generated vector represents the semantic meaning of the original data.

---

## 3. AI Vector Search

AI Vector Search finds relevant information by comparing the semantic meaning of vectors.

Traditional keyword search mainly looks for matching words, while vector search can find results with similar meaning even when different words are used.

Example:

```text
Query:
"Customer cannot login"

Document:
"User is unable to sign in"
```

Although the words are different, their meanings are similar, so vector search can identify the document as relevant.

---

## 4. Similarity Search

Similarity Search finds vectors that are most similar to a given query vector.

```text
Query Vector
     ↓
Compare with Vector Data
     ↓
Find Similar Vectors
     ↓
Return Relevant Results
```

The goal is to find the most relevant information based on semantic similarity.

---

## 5. Outlier Search

Outlier Search identifies vectors that are significantly different from other vectors in a dataset.

Example:

```text
Normal Data:
A A A A A A A

Outlier:
        X
```

The outlier may represent unusual, abnormal, or unexpected data.

---

## 6. Vector Distance

Vector distance measures how far two vectors are from each other.

Generally:

```text
Smaller Distance → More Similar
Larger Distance  → More Different
```

Common distance or similarity measures include:

* Euclidean Distance
* Manhattan Distance
* Cosine Similarity / Distance

---

## 7. VECTOR Data Type

Oracle Database provides the `VECTOR` data type for storing numerical vectors and embeddings directly inside the database.

Example:

```text
EMBEDDING VECTOR
```

A four-dimensional vector can contain:

```text
[0.2, 0.5, -0.1, 0.8]
```

The number of numerical values represents the vector dimensions.

---

## 8. Vector Index

A vector index is a specialized database index designed to make vector similarity searches more efficient, especially when dealing with large numbers of vectors.

```text
Large Vector Dataset
        ↓
   Vector Index
        ↓
 Efficient Search
```

A vector index does not generate the vector. It helps search existing vectors efficiently.

---

## 9. Nearest Neighbor Search

A nearest neighbor is a vector that is close or similar to another vector.

Nearest Neighbor Search finds the vectors closest to a query vector.

```text
Query Vector
     ↓
Nearest Vector 1
Nearest Vector 2
Nearest Vector 3
```

It is commonly used for recommendation systems, semantic search, document retrieval, and AI applications.

---

# Approximate Vector Search

When a dataset contains millions or billions of vectors, comparing the query with every vector can be expensive.

Approximate Nearest Neighbor (ANN) techniques improve search speed by avoiding exhaustive comparison with every vector.

Two important approaches are:

* HNSW
* IVF Flat

---

## 10. HNSW

**HNSW = Hierarchical Navigable Small World**

HNSW is a graph-based approximate nearest neighbor indexing technique.

Vectors are represented as nodes, and connections are created between nearby or similar vectors.

```text
       A
      / \
     B---C
      \ /
       D
```

During search, the algorithm navigates through the graph to find nearby vectors efficiently.

### Key Point

**HNSW = Graph-based Vector Index**

---

## 11. IVF Flat

**IVF = Inverted File**

IVF organizes vectors into different inverted lists or clusters.

```text
Vectors
   ↓
-------------------------
| List 1 | List 2       |
| List 3 | List 4       |
-------------------------
```

During a search, the relevant lists are selected and the vectors within those lists are searched.

In **IVF Flat**, actual distance calculations are performed on vectors in the selected lists.

### Key Point

**IVF Flat = Cluster/List-based Vector Index**

---

## 12. HNSW vs IVF Flat

| HNSW                                | IVF Flat                            |
| ----------------------------------- | ----------------------------------- |
| Graph-based                         | Cluster/List-based                  |
| Uses connections between vectors    | Organizes vectors into lists        |
| Navigates through a graph           | Selects relevant lists              |
| Approximate nearest neighbor search | Approximate nearest neighbor search |

### Remember

```text
HNSW → Graph
IVF  → Lists / Clusters
```

---

# Scale-Out in AI Vector Search

Scale-out means increasing capacity and performance by using multiple nodes, databases, or distributed resources instead of depending only on one system.

Oracle AI Database supports different approaches for scaling large vector workloads.

---

## 13. AI Vector Search Scale-Out with RAC

**RAC = Real Application Clusters**

Oracle RAC allows multiple database instances to work together with the same database.

```text
             Database
                 |
       ---------------------
       |         |         |
     Node 1    Node 2    Node 3
```

Multiple nodes can handle database workloads, providing scale-out capabilities.

### Key Point

**RAC → Multiple Database Nodes → Scale-Out**

---

## 14. AI Vector Search Scale-Out with Exadata Smart Storage

Oracle Exadata provides intelligent processing closer to the storage layer.

For supported operations, vector-related processing can be pushed closer to where the data is stored. This can reduce unnecessary data movement and database-server processing.

```text
Traditional:

Storage → Database Server → Processing


Exadata:

Storage
   ↓
Processing closer to data
   ↓
Database Server
```

### Key Point

**Exadata Smart Storage → Processing closer to the data**

---

## 15. AI Vector Search Scale-Out with Partitioning

Partitioning divides a large database table into smaller partitions.

```text
Large Vector Table
        ↓
-------------------------
P1 | P2 | P3 | P4
-------------------------
```

Partitioning helps organize and manage large datasets and can improve query performance when appropriate partition pruning is possible.

### Important Difference

Database partitioning is **not the same as IVF**.

```text
Database Partitioning
        ↓
Table/Data Organization
        ↓
P1 | P2 | P3 | P4
```

While:

```text
IVF
 ↓
Vector Index Organization
 ↓
Lists / Clusters
```

### Key Point

**Partitioning = Database/Table-level organization**

**IVF = Vector-index-level organization**

---

## 16. AI Vector Search Scale-Out with Sharding

Sharding distributes data across multiple independent database shards.

```text
             Large Vector Dataset
                     |
          -------------------------
          |           |           |
       Shard 1     Shard 2     Shard 3
```

A vector similarity search can be executed across multiple shards, potentially in parallel, and the results can be combined.

```text
              Query Vector
                   |
       -------------------------
       |           |           |
    Shard 1     Shard 2     Shard 3
       ↓           ↓           ↓
    Results     Results     Results
       \           |           /
        \          |          /
             Final Results
```

### Key Point

**Sharding = Distributing data across multiple database shards for scale-out.**

---

## 17. Sharding + Vector Indexes

Sharding and vector indexes are not alternatives.

They can work together.

```text
          Sharded Vector Data
                  |
      ---------------------------
      |            |            |
    Shard 1      Shard 2      Shard 3
      |            |            |
    HNSW          IVF          HNSW
```

Sharding provides distributed scale-out, while HNSW or IVF provides efficient vector search.

### Remember

```text
Sharding → Scale-Out
HNSW / IVF → Efficient Vector Search
```

---

# Full Integration with Oracle Database

## 18. Full Integrated AI Vector Search

One of the major advantages of Oracle AI Database is that vector search is integrated into the database.

Vector data can exist together with traditional enterprise data instead of requiring a completely separate vector database.

```text
              Oracle AI Database
                     |
      --------------------------------
      |        |        |            |
   Relational  JSON    Graph       Vector
      Data     Data    Data        Search
      |        |        |            |
      -------- Same Database --------
```

This allows applications to combine vector similarity search with normal database operations and other data types.

For example:

```text
Semantic Search
      +
Customer Information
      +
Transaction Data
      +
Filtering
```

can be handled within the database environment.

### Key Point

**Full Integration = Vector Search is natively integrated with the database and can work with enterprise data.**

---

# 19. AI Vector Search with Highlighting

Highlighting helps users identify relevant parts of returned text.

Example:

Query:

```text
Customer cannot login
```

Result:

```text
The customer was unable to login to the application.
```

The relevant text can be highlighted for the user:

```text
The customer was unable to LOGIN to the application.
```

Highlighting improves the presentation of search results and helps users understand why a text result is relevant.

### Important Difference

```text
Vector Search → Finds relevant results
Highlighting  → Shows relevant text/parts to the user
```

---

# Complete AI Vector Search Flow

```text
Original Data
      ↓
Embedding Model
      ↓
Numerical Vector
      ↓
VECTOR Data Type
      ↓
Vector Index
      ↓
HNSW / IVF
      ↓
AI Vector Search
      ↓
Similarity / Nearest Neighbor Search
      ↓
Relevant Results
      ↓
Highlighting
      ↓
User
```

For large-scale environments:

```text
                 AI Vector Search
                       |
       ---------------------------------
       |               |               |
      RAC        Partitioning       Sharding
       |               |               |
       ---------------------------------
                       |
                  Scale-Out
                       |
                  Exadata Support
```

# Quick Revision

| Topic             | Main Purpose                                    |
| ----------------- | ----------------------------------------------- |
| Vector            | Represents data numerically                     |
| Embedding         | Generates vectors                               |
| AI Vector Search  | Finds semantically relevant data                |
| Similarity Search | Finds similar vectors                           |
| Outlier Search    | Finds unusual vectors                           |
| Vector Distance   | Measures distance between vectors               |
| VECTOR            | Stores vectors in database                      |
| Vector Index      | Makes vector search efficient                   |
| HNSW              | Graph-based ANN                                 |
| IVF Flat          | List/cluster-based ANN                          |
| Partitioning      | Divides database table/data                     |
| RAC               | Scale-out using multiple database nodes         |
| Sharding          | Distributes data across database shards         |
| Exadata           | Moves supported processing closer to storage    |
| Highlighting      | Shows relevant text to users                    |
| Full Integration  | Vector search works natively with database data |

## Final Memory Trick

```text
VECTOR      → Store
EMBEDDING   → Generate
DISTANCE    → Measure
SEARCH      → Find
HNSW        → Graph
IVF         → Lists/Clusters
PARTITION   → Divide Table
RAC         → Multiple Nodes
SHARDING    → Distributed Data
EXADATA     → Processing Near Storage
HIGHLIGHT   → Show Relevant Text
```
