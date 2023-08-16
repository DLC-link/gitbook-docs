# DLC Attestors

**Introduction**

DLC (Discreet Log Contracts) attestors are vital third-party services that verify the outcome of a DLC. They enable programmatic, conditional Bitcoin payments by facilitating communication between the Bitcoin ledger and other systems.

**High-Level Overview of Attestors**

1. **Listen to Smart Chains**: Attestors listen to smart-chain events, such as Ethereum or Stacks, through an API for creating or closing DLCs.
2. **Announce DLC Events**: Announcements of new DLC events occur via a public webpage using JSON, where a set of signatures are provided and used to create the CETs (Contract Execution Transactions).
3. **Attest to Outcomes**: Attestors sign a value based on the correct output from a data source (e.g., smart contracts on other blockchain networks) to attest to one of the outcomes.

**How are DLC Attestors different from Bitcoin Oracles?**

* **Oracles** are seen as the trusted source of truth outside the context of the DLC and the Bitcoin Blockchain.
* **Attestors** simply attest (sign with a private key) to an outcome based on this data.

In DLC.Link architecture, smart blockchain contracts (e.g. Ethereum, Stacks) act as the source of truth (Oracle), and the DLC Bitcoin Attestors simply sign this data.

**Attestor DLC Signing**

1. **Generation of Pre-Signatures**: When a DLC is created, the attestor pre-signs for each potential outcome.
2. **Final Outcome Signing**: When it's time to close a DLC, the attestor signs one of the possible outcomes, enabling either Bitcoin user to finalize the payout.

**Running Attestor Nodes**

* **Architecture**: Nodes are deployed using technologies like Docker, combining Rust and JS for optimal performance.
* **Onboarding Process**: Parties interested in running attestors in DLC.Link's network must be whitelisted by our team.
* **Running the Software**: Specific configurations, downloading, and running Docker files are necessary.
* **Key Management**: Critical to the solution, options include Cloud HSM, Cloud KMS, HashiCorp Vault, Local Key Management, or a blended solution.

**Health Reporting / Reputation Score**

Attestors' performance is measured by factors such as:

* Number of open DLC events.
* Response times to create events.
* Number of incorrectly attested outcomes.

Key consistency, response time, and total availability are vital for running a healthy attestor.

**Incentive Structure**

* **Quarantine**: Underperforming attestors can be placed into a warning/quarantine state.
* **Slashing**: Punishing badly performing or malicious nodes through slashing mechanisms.

#### Conclusion

DLC Attestors form an integral part of the DLC framework, performing a crucial role in listening to smart chain events, announcing DLC events, and attesting to outcomes. Their architecture, key management, performance metrics, and incentives ensure a trustworthy and resilient system for DLCs, opening the door for trustless Bitcoin financial applications.
