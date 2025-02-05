# Cisco NetFlow 9.0

A typical flow monitoring setup (using NetFlow) consists of three main components:
- Flow exporter: aggregates packets into flows and exports flow records towards one or more flow collectors.
- Flow collector: responsible for reception, storage and pre-processing of flow data received from a flow exporter.
- Analysis application: analyzes received flow data in the context of intrusion detection or traffic profiling, for example.

### Best Practice / Highlights
1. NetFlow is based on 7 key fields: (If one field is different, a new flow is created in the flow cache.)
   - Source IP address
   - Destination IP address
   - Source port number
   - Destination port number
   - Layer 3 protocol type (ex. TCP, UDP)
   - ToS (type of service) byte
   - Input logical interface
2. It is best practice to use a NetFlow “source interface” that would never go down such as a loopback interface.

### Flexible NetFlow
Define custom fields other than the 7 key fields.
