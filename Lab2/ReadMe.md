Design for Testability (DFT): Scan Test Insertion

This repository contains the lab work for implementing Design for Testability (DFT) using Scan Test Insertion techniques. The project demonstrates the process of improving testability in a UART transceiver design written in Verilog HDL.
Introduction

In modern semiconductor designs, as technology scales down (e.g., IBM's 2nm technology), ICs become more prone to faults due to their smaller die sizes and higher complexity. Design for Testability (DFT) addresses these challenges by adding additional logic to improve the testability of integrated circuits.

This lab focuses on Scan Test, a widely used DFT technique to detect manufacturing faults in silicon devices. By replacing standard flip-flops with scannable flip-flops and creating a scan chain, we enable efficient monitoring of the design's internal states, ensuring robust testing while minimizing additional costs.
Project Overview
Scan Test Flow

    Replace flip-flops with scannable flip-flops.
    Create a scan chain to monitor internal states.
    Use Automatic Test Pattern Generator (ATPG) tools to generate test vectors and detect faults.
    Insert Scan Test logic following the general DFT insertion flow.

    Key Objectives

    Learn how to insert Scan Test logic into an IC design.
    Explore the trade-offs between testability and additional design logic.
    Generate and analyze reports to evaluate the efficiency of the scan insertion process.

    Tools Used

    Design Software: Cadence Tools (e.g., Genus)
    Libraries: 45nm GPDK Technology Libraries
    Language: Verilog HDL
