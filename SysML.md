---
created: 2026-04-08
tags:
  - note
  - journal
---
**Background**

At its core, SysML (Systems Modeling Language) is a specialized dialect of UML (Unified Modeling Language). While UML was designed primarily for software engineering, SysML was stripped of software-centric baggage and expanded to handle the physical, hardware, and operational constraints of complex systems.

If you are a mechanical, electrical, or software engineer, you can think of SysML as a **centralized, visual database** that replaces the "document-centric" approach to engineering. Instead of having a 200-page Word document for requirements, a separate Excel sheet for an interface matrix, and various CAD files, SysML integrates these into a single model where every element is linked.

Here is how SysML organizes the system across its four primary pillars:

### 1. Structure (The "What")
While you might use CAD for physical geometry, SysML uses **Block Definition Diagrams (bdd)** and **Internal Block Diagrams (ibd)** to define the logical architecture.
*   **bdd:** Defines the hierarchy. It tells you that a "Power Subsystem" consists of a "Battery," a "Voltage Regulator," and a "Distribution Board."
*   **ibd:** Defines the connectivity. It specifies the "Flows" between blocks—such as 28V DC electricity moving from the battery to the regulator via a specific port.

### 2. Behavior (The "How")
This describes the system's logic and timing without committing to a specific implementation.
*   **State Machine Diagrams:** Define the operational modes (e.g., *Standby $\rightarrow$ Active $\rightarrow$ Fault*).
*   **Activity Diagrams:** Essentially advanced flowcharts that map the sequence of actions and the data/material flowing between them.
*   **Sequence Diagrams:** Map the chronological exchange of messages between different components over time.

### 3. Requirements (The "Why")
In a traditional workflow, requirements are text in a database (like DOORS). In SysML, requirements are **model elements**. This allows for "Traceability":
*   You can draw a relationship line from a high-level requirement (e.g., "The system shall operate for 10 hours") directly to the Block that satisfies it (the Battery) and the Test Case that verifies it. If the battery spec changes, you can instantly see which requirements are impacted.

### 4. Parametrics (The "How Much")
This is the most "engineering-heavy" part of SysML. Parametric diagrams allow you to bind mathematical equations to the system structure. 
*   Instead of just saying "the system must be light," you link a mathematical constraint (e.g., $\sum \text{mass}_{\text{components}} \leq 50\text{kg}$) to the blocks in your structure. If a mechanical engineer increases the weight of a bracket in the model, the parametric diagram can flag that the total system mass now violates the requirement.

### The Bottom Line for You
As a domain engineer, SysML is the "Single Source of Truth." It prevents the common failure where the software team assumes the hardware provides a certain signal, but the hardware team designed it differently. By forcing these interfaces and behaviors into a formal model, the gaps are identified during the design phase rather than during integration and test.


**SysML v1 vs. SysML v2**

The transition from SysML v1 to SysML v2 is not just a minor update; it is a fundamental paradigm shift. While v1 was a **profile of UML** (a set of specialized symbols added to a software language), v2 is a **standalone language** with its own dedicated metamodel and formal semantics.

Here is the breakdown of the primary differences across four key dimensions:

### 1. Foundations: Profile vs. Native Language
*   **SysML v1:** It was essentially "UML with a makeover." Because it relied on the UML metamodel, it inherited a lot of software-centric complexity and limitations. If you wanted to do something that UML wasn't designed for, you had to create "stereotypes" (essentially labels) to force the language to behave like a systems engineering tool.
*   **SysML v2:** It is built from the ground up. It has its own **Kernel Model (KerML)**, which provides a rigorous mathematical foundation. This means the language is more precise, less ambiguous, and specifically designed for systems engineering rather than software objects.

### 2. Representation: Graphical vs. Textual
*   **SysML v1:** It was almost entirely graphical. To change a relationship, you had to drag a line between boxes in a tool. While this is intuitive, it makes "version control" (like using Git) nearly impossible because the files are complex XMLs or binaries.
*   **SysML v2:** It introduces a **formal textual notation**. You can now write your system architecture as code. 
    *   *Example:* You can define a part and its properties in a text file, which the tool then automatically renders as a diagram. 
    *   This allows engineers to use software development best practices: branching, merging, and diffing changes in a text-based format.

### 3. Modeling Precision and Semantics
*   **SysML v1:** Much of the meaning was "informal." Two different engineers might interpret a line on a diagram differently because the underlying rules were loose.
*   **SysML v2:** It introduces **stronger semantics**. The language is designed to be "computable." Because the underlying logic is more formal, it is much easier to perform automated analysis, such as:
    *   **Model Execution:** Running the model to simulate behavior more accurately.
    *   **Automated Verification:** Checking if the design mathematically satisfies the requirements without needing a separate external tool.

### 4. Key Feature Enhancements
| Feature | SysML v1 | SysML v2 |
| :--- | :--- | :--- |
| **Requirements** | Textual blocks linked to elements. | First-class citizens with formal constraints. |
| **Interfaces** | Defined via Ports and Flow Properties. | Simplified "Interface" and "Port" definitions that are more intuitive. |
| **Variability** | Difficult to model "Product Lines" (e.g., a basic vs. deluxe version). | Built-in support for variability and optionality. |
| **Geometry** | Very limited; relied on external CAD. | Better integration for spatial and geometric constraints. |

### Summary for the Engineer
If **SysML v1** was like drawing a blueprint by hand on a piece of paper (where the "rules" were mostly in the engineer's head), **SysML v2** is like using a parametric CAD tool. The "drawing" is just a visualization of a rigorous, underlying database of logic and math. It moves the industry from **"Drawing Diagrams"** to **"Engineering Models."**