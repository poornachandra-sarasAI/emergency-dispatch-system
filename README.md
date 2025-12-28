# Emergency Response Dispatch System

## Project Overview

This project implements an **Emergency Response Dispatch System** that efficiently manages and routes emergency services (Medical and Fire) to incident locations in a city. The system uses various data structures and algorithms to optimize emergency response times and infrastructure planning.

## Features

- **Graph-based City Modeling**: Represents the city as a weighted graph with locations as nodes and roads as edges
- **Shortest Path Finding**: Uses Dijkstra's algorithm to find the nearest emergency facility for each incident
- **Incident Prioritization**: Implements a priority queue (min-heap) to process incidents based on severity levels
- **Infrastructure Planning**: Uses Minimum Spanning Tree (MST) to determine minimum road infrastructure needed
- **Traversal Algorithms**: Implements DFS and BFS to find unreachable locations and visualize graph traversal
- **Incident History Tracking**: Uses a Stack data structure to maintain a record of processed incidents
- **Efficient Lookups**: Implements hashing for fast location name-to-ID mapping and emergency type categorization
- **Visualizations**: Generates visual representations of the city map, MST, and incident locations

## File Architecture

```
Emergency_Response_Dispatch_System/
│
├── datasets/                          # Data files
│   ├── location.csv                  # City locations with coordinates and categories
│   ├── roads.csv                     # Road network with distances and traffic delays
│   ├── emergencies_facilities.csv     # Emergency facilities (Hospitals and Fire Stations)
│   └── incidents_sample.csv          # Sample emergency incidents
│
├── notebooks/                         # Jupyter notebooks
│   └── main.ipynb                    # Main implementation notebook
│
├── screenshots/                       # Generated visualizations
│   ├── City_Map_with_Weights.png
│   ├── Minimum_Spanning_Tree.png
│   ├── Minimum_Spanning_Tree_Highlighted_on_Graph_G.png
│   └── Network_with_Incident_Locations.png
│
├── Project_DSA .docx.pdf             # Project documentation
└── README.md                         # This file
```

## Dataset Descriptions

### 1. `location.csv`
Contains information about all locations in the city:
- **id**: Unique location identifier (e.g., L1, L2)
- **name**: Location name (e.g., "Connaught Place", "AIIMS Delhi")
- **latitude**: Geographic latitude coordinate
- **longitude**: Geographic longitude coordinate
- **category**: Type of location (Market, Hospital, Tourist, Transport, etc.)

### 2. `roads.csv`
Defines the road network connecting locations:
- **source_id**: Starting location ID
- **target_id**: Destination location ID
- **distance_km**: Distance in kilometers
- **avg_traffic_delay_min**: Average traffic delay in minutes

### 3. `emergencies_facilities.csv`
Lists all emergency facilities available:
- **location_id**: Location ID of the facility
- **facility_type**: Type of facility (Hospital or FireStation)
- **name**: Facility name
- **capacity_level**: Capacity level (High, Medium, Low)

### 4. `incidents_sample.csv`
Contains sample emergency incidents:
- **incident_id**: Unique incident identifier (e.g., INC1, INC2)
- **location_id**: Location where the incident occurred
- **emergency_type**: Type of emergency (Medical or Fire)
- **severity_level**: Severity (High, Medium, Low)
- **description**: Brief description of the incident

## Main Notebook: `main.ipynb`

The `main.ipynb` notebook contains the complete implementation of the Emergency Response Dispatch System. It is organized into the following sections:

### 1. **Data Loading and Initialization**
- Loads location and road data from CSV files
- Initializes a NetworkX graph structure
- Creates category mappings for locations

### 2. **Graph Construction**
- **Node Addition**: Adds all locations as nodes in the graph with their names as attributes
- **Edge Addition**: Creates weighted edges between connected locations
  - Edge weight = `distance_km + avg_traffic_delay_min/3`
  - This weight represents the total travel cost considering both distance and traffic

### 3. **Traversal Algorithms**
- **Unreachable Locations Function**: Uses Depth-First Search (DFS) to find locations that cannot be reached from a given source
- Implements graph traversal to analyze connectivity

### 4. **Best Facility Finder**
- **Function**: `find_best_facility(incident_location_id, emergency_type)`
- Uses **Dijkstra's Algorithm** to find the shortest path from an incident location to the nearest appropriate emergency facility
- Returns:
  - `facility_location_id`: ID of the nearest facility
  - `path`: List of location IDs representing the shortest route
  - `min_dist`: Minimum travel cost to reach the facility
- Handles unreachable facilities by skipping them

### 5. **Minimum Spanning Tree (MST)**
- Uses Kruskal's/Prim's algorithm (via NetworkX) to find the MST
- Identifies the minimum road infrastructure needed to keep all emergency-related locations connected
- Helps in infrastructure planning and cost optimization

### 6. **Hashing Implementation**
- **Location Name Mapping**: Creates a hash map (`locations_map`) for O(1) lookup of location IDs by name
- **Facility Categorization**: Uses dictionary hashing to categorize facilities by emergency type (Medical/Fire)

### 7. **Priority Queue for Incident Management**
- Implements a **min-heap** using Python's `heapq` module
- Prioritizes incidents based on:
  - **Primary**: Severity level (High=1, Medium=2, Low=3)
  - **Secondary**: Timestamp (FIFO for same severity)
- Uses `namedtuple` for structured incident data:
  - `priority`: Severity level
  - `timestamp`: Time of incident creation
  - `incident_id`: Unique identifier
  - `loc_id`: Location ID
  - `emergency_type`: Type of emergency

### 8. **Stack for Incident History**
- Implements a custom **Stack class** with methods:
  - `push()`: Add processed incident to history
  - `pop()`: Remove and return last incident
  - `peek()`: View last incident without removing
  - `is_empty()`: Check if stack is empty
  - `display()`: Show all incidents in stack
- Maintains a record of all processed incidents with their dispatch details

### 9. **Incident Processing**
- **Function**: `process_incident()`
- Pops the highest priority incident from the priority queue
- Finds the best facility using `find_best_facility()`
- Pushes the processed incident (with route and cost) to the history stack
- Displays dispatch information

### 10. **Visualizations**
The notebook generates several visualizations:
- **City Map with Weights**: Shows the complete graph with all nodes, edges, and edge weights
- **Minimum Spanning Tree**: Displays the MST highlighting the essential connections
- **MST Highlighted on Graph G**: Overlays the MST on the full graph for comparison
- **Network with Incident Locations**: Color-coded visualization showing:
  - Medical facilities (Green)
  - Fire stations (Red)
  - Regular locations (Gray)
  - Incident locations (Yellow stars)

## Data Structures and Algorithms Used

### Data Structures
1. **Graph** (NetworkX): Represents city network
2. **Priority Queue (Min-Heap)**: Incident prioritization
3. **Stack**: Incident history tracking
4. **Hash Map/Dictionary**: Fast location lookups and categorization
5. **Named Tuple**: Structured incident data

### Algorithms
1. **Dijkstra's Algorithm**: Shortest path finding
2. **Depth-First Search (DFS)**: Graph traversal and connectivity analysis
3. **Breadth-First Search (BFS)**: Graph traversal (mentioned for future use)
4. **Minimum Spanning Tree (MST)**: Infrastructure optimization
5. **Heap Operations**: Priority queue management

## Installation and Requirements

### Prerequisites
- Python 3.7 or higher
- Jupyter Notebook

### Required Libraries
```bash
pip install pandas networkx matplotlib
```

### Libraries Used
- **pandas**: Data manipulation and CSV reading
- **networkx**: Graph creation and algorithm implementation
- **matplotlib**: Visualization and plotting
- **heapq**: Priority queue implementation (built-in)
- **collections**: Named tuple support (built-in)
- **time**: Timestamp generation (built-in)

## Usage

1. **Open the notebook**:
   ```bash
   jupyter notebook notebooks/main.ipynb
   ```

2. **Run cells sequentially** to:
   - Load and process data
   - Build the graph network
   - Process incidents from the priority queue
   - Generate visualizations

3. **Process incidents**:
   - Incidents are automatically loaded into the priority queue
   - Call `process_incident()` to dispatch the highest priority incident
   - View processed incidents in `inc_history.stack`

4. **View visualizations**:
   - All visualizations are saved in the `screenshots/` directory
   - Generated images show the city network, MST, and incident locations

## Key Functions

### `unreachable_locations(G, source)`
Finds all locations unreachable from a given source using DFS.

**Parameters**:
- `G`: NetworkX graph
- `source`: Starting location ID

**Returns**: List of unreachable location IDs

### `find_best_facility(incident_location_id, emergency_type)`
Finds the nearest appropriate emergency facility for an incident.

**Parameters**:
- `incident_location_id`: Location where incident occurred
- `emergency_type`: Type of emergency ("Medical" or "Fire")

**Returns**: Tuple of (facility_id, path, cost) or (None, None, None) if no facility found

### `process_incident()`
Processes the highest priority incident from the queue and dispatches it to the nearest facility.

**Returns**: None (prints dispatch information)

## Example Output

```
Pushed ('INC1', 'L1', 'Medical', 'L2', ['L1', 'L2'], 10.33)
Incident INC1 can go to facility L2 with cost 10.33
```

## Project Objectives

This project demonstrates the practical application of:
- Graph theory in real-world routing problems
- Priority queues for task scheduling
- Stack data structure for history tracking
- Hashing for efficient data retrieval
- Algorithm optimization for emergency response systems

## Future Enhancements

- Implement BFS visualization
- Add real-time incident updates
- Support multiple emergency types
- Implement dynamic traffic updates
- Add capacity-based facility selection
- Create a web-based dashboard

## Author

R Mohan Poornachandra

## License

This project is for educational purposes.

