Wireshark is an open-source, cross-platform network packet analyser tool that can sniff and investigate live traffic, by inspecting packet captures (PCAP).

On opening of the interface, we can see several parts available:
- Toolbar: allows to access different menus for packet snifing and processing
- Display Filter Bar: to make queries and filtering 
- Recent Files: quick access for previously opened files
- Capture Files and Interfaces: list of available sniffing points and network interfaces
- Status Bar: shows status of the tool and packet information.

When we open a PCAP file, the interface changes and several parts can be seen:
- Packet List Pane: shows the lust of all intercepted packets with source/destination addresses, protocol, and paket info. We can click on a packet to investigate further
- Packets Details Pane: detailed breakdown of a packet
- Packet Bytes Panes: hex and decoded ASCII representation of the selected packet

The tool also allows to colour packets based on specific conditions or protocols. Permanent rools can be created by going in the `View -> Coloring` menu. Temporary coloring is done by going in the `View -> Conversation Filter` menu.

The actual traffic sniffing is done when clicking on the blue shark button.

PCAP files can also be merged when going in the `File -> Merge` menu.

The file details can also be accessed, which is useful when dealing with many files we need to categorize and prioritize. It can be accessed by either going in the `Statistics -> Capture File Properties` or clicking on the `PCAP` button.

It is possible to dissect a packet into the different protocols it relies on, for example according to the OSI layers.

When browsing the packets, Wireshark assigns a umber to each packet to help the analysis process. There is a "Go to" search bar we can use to access a specific packet number. Filtering can also be applied by by enabling the Find Packet option in the `Edit -> Find Packet` menu. The search accepts four types of inputs:
- Display filter
- Hex
- String
- Regex
And provides three search fields:
- packet list
- packet details
- packet bytes

After searching for specific packets, it is possible to mak them by right clicking. The packet s matching the one you marked will be displayed in black, regardless of the color rule that was applied to them. These marks are removed once the file is closed.

In addition you can add comments to the packets to help for later analysis. These comments remain after closing the file.

It is also possible to export packets and files, when the data transmitted corresponds to file data in the second scenario.

The time display format can also be changed.

Wireshark also detects the state of used protocols to help identify anomalies. The colors are the following:
- Blue: information on usual workflow
- Cyan: notable events like application error code
- Yellow: Warnings like unusual error codes or problem statements
- Red: Problems like malformed packets

Information groups allow to gather errors/specific packets into categories. Some common groups are:
- Checksum: gather checksum errors
- Comment: Packet comment detection
- Deprecated: for deprecated protocol usage
- Malformed: malformed packet detection

These elements can be accessible by going inside the `Analyse -> Expert Information` menu.

Wireshark implements a golden rule for analysts which is that "if you can click on ut, you can filter and copy it".

After clicking on an element we can apply a filter based on the specific protocol or value that has been clicked by going in the `Analysis -> Apply as a filter` menu and clicking on `Selected`.

This way of filtering is basic, and does not allow to filter according to the relations of a packet. For example, if we want to filter the TCP packets that are part of the same TCP session than the packet we have clicked, we need to go in the `Analyse -> Conversation Filter` menu, and then select the given protocol we want the relation to be based on. Wireshark will automatically update the filter field.

In addition of this way of filtering, coloring can also be applied by going in the `View -> Colorize Conversation` menu.

Successive filters can also be applied unsing this "clicking" method. This can be done by using the `Prepare as a filter` field. The filtering will not be applied directly but it will update the filtering command. It allows to accumulate different filters on the go and once satisfied, hitting enter will apply all of them at once.

Columns can be added by hitting the `Analyse -> Apply as a Column` menu field.

When dealing with a lot of successive data, for example when analyzing the TCP exchanges between a client and a server, the entire discussion can be rendered at once, by using the `Analyse -> Follow TCP/UDP/HTTP Stream` option.

The packet capture can also be done using [[Tcpdump|tcpdump]]