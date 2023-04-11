---
title: "Computer Networks and The Internet"
---

## 1. What is the Internet?
### A Nuts-andBolts Description
 The Internet is a computer network that interconnects bilions of computing devices throughout the world. In Internet jargon, all of these devices are called hosts or end systems. 

 End systems are connected together by a network of **communication links** and **packet switches**. Differenct links can transmit data at different rates, with the **transmission rate** of a link measured in bits/second. When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as **packets** in the jargon of computer networks, are then sent through the network to the destination end system, where they are reassembled into the original data. 
 
 Packet switches come in many shapes and flavors, but the two most prominent types in today's Internet are **routers** and **link-layer switches**. Both types of switches forward packets toward ultimate destinations. Link-layer switches are typically used in **access networks**, while routers are typically used in the **network core**. The sequence of communication links and packet switches traversed by a packet from the sending end system to the receiving end system is known as a **route** or **path** through the network. 
<img src="../assets/isp.jpeg" width = "800vw" height = "600vw">

 End system access the Internet through **Internet Service Providers(ISPs)**, including residential USPs such as local cable or telephone companies; Each ISP is in itself a network of packet switches and communication links. ISPs provide a variety of types of network access to the end systems, including residential broadband access.

 End systems, packet switches, and other pieces of the Internet run **protocols** that control the sending and receiving of information within the Internet. The **Transmission Control Protocol(TCP)** and the **Internet Protocol(IP)** are two of the most important protocols in the Internet. The IP protocol specifies the format of the packets that are sent and received among routers and end systems. The Internet's principal protocols are collectively known as **TCP/IP**.

### A Services Description
End system attached to the Internet provide a **socket interface** that specifies how a program running on one end system asks the Interenet infrastructure to deliver data to a specific destination program running on another end system. This Internet socket interface is a set of rules that the sending program must follow so that the Internet can deliver the data to the destination program.

### What is Protocol?
All activity in the Internet that involves two or more communicating remote entities is governed by a protocol.
> A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

<hr>

## 2. The Network Edge
Hosts are sometimes further divided into two categories : **clients** and **servers**.

### 2.1 Access Network
Today, the two most prevalent types of broadband residential access are **digital subscriber line(DSL)** and cabel. 