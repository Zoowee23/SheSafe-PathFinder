# SheSafe-PathFinder
# PathfinderApp 🚦

## 🧠 Project Description

**PathfinderApp** is a Java-based GUI application designed to compute the *safest path* between two nodes in a graph using risk scores as edge weights. It simulates a real-world application like emergency evacuation planning, secure routing, or urban navigation where safety is prioritized over distance.

The graph's nodes and edges are stored in a **MySQL** database and loaded dynamically at runtime. Each edge has a **risk score** calculated from various risk parameters (`CSS`, `SLF`, `PPI`, `PPS`). Using **Dijkstra's algorithm**, the app determines the path with the lowest total risk.

With an intuitive **Java Swing GUI**, users can:

- Enter source and destination nodes
- View the safest route and its total risk score
- Trigger **Emergency Mode** to view local emergency contacts
- Rate the safety of locations
- View the **Top 5 Safest Locations** as rated by users

---

## ⚙️ Features

### 🔍 Safest Path Finder
- Computes the safest path between any two locations based on cumulative risk score.
- Utilizes Dijkstra’s algorithm to ensure optimal path selection.

### 🚨 Emergency Mode
- Users can click an **"Emergency" button**.
- Based on the user’s **current location**, emergency contacts (like police, hospital, fire station) for that area are fetched from the database and displayed.

### ⭐ Location Safety Rating
- Users can **rate the safety** of a location (1 to 5 stars).
- Ratings are stored in the database.
- A section displays the **Top 5 Safest Locations** based on average user ratings.

### 🧭 User-Friendly Interface
- Java Swing-based interface for easy input and visual feedback.
- Scrollable result area with clearly formatted output.

---

## 🗃️ Data Structures Used

### 1. **Graph (Adjacency List)**
- `Map<String, List<Edge>>`: Represents the entire network of nodes and edges.
- Efficient for traversal and neighbor lookups in sparse graphs.

### 2. **Edge**
- Fields: `to` (destination node), `riskScore` (sum of multiple factors)
- Each connection between nodes is weighted with a cumulative risk score.

### 3. **Priority Queue**
- Used in Dijkstra’s algorithm to always pick the node with the current lowest risk.
- Efficient pathfinding with dynamic updates of minimum scores.

### 4. **HashMaps**
- `dist`: Stores minimum risk from source to all other nodes.
- `prev`: Stores the predecessor node for reconstructing the path.

---

## 🧩 Technologies Used

- **Java 11+**
- **Swing (GUI)**
- **JDBC (Java Database Connectivity)**
- **MySQL**

---

## 🛠️ Setup & Installation

### 1. Database Setup

Create a MySQL database `pathfinder_db` with tables:

```sql
CREATE TABLE nodes (
    NodeID INT PRIMARY KEY,
    NodeName VARCHAR(50)
);

CREATE TABLE edges (
    FromNode INT,
    ToNode INT,
    CSS DOUBLE,
    SLF DOUBLE,
    PPI DOUBLE,
    PPS DOUBLE,
    FOREIGN KEY (FromNode) REFERENCES nodes(NodeID),
    FOREIGN KEY (ToNode) REFERENCES nodes(NodeID)
);

CREATE TABLE emergency_contacts (
    Location VARCHAR(50),
    PoliceContact VARCHAR(20),
    FireContact VARCHAR(20),
    HospitalContact VARCHAR(20)
);

CREATE TABLE location_ratings (
    Location VARCHAR(50),
    Rating INT
);
