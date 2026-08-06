# Threat Hunting Gap Register

## GAP-001: Important Fields Stored Inside Message

**Impact:** Every hunt requires repeated `rex` extraction, increasing query complexity and inconsistency.

**Improvement:** Use normalized Windows event ingestion and reusable field extractions.

**Priority:** High

---

## GAP-002: Manual Collection

**Impact:** Data is not continuous and cannot prove what occurred outside exported windows.

**Improvement:** Deploy a supported real-time collection architecture.

**Priority:** High

---

## GAP-003: Limited Network Visibility

**Impact:** Endpoint DNS and connection events do not provide full packet or organization-wide context.

**Improvement:** Correlate with DNS resolver, proxy, firewall, or Zeek logs.

**Priority:** Medium

---

## GAP-004: Small Baseline

**Impact:** Rare activity cannot be reliably classified as anomalous.

**Improvement:** Collect multiple weeks of representative activity from comparable endpoints.

**Priority:** High

---

## GAP-005: Possible Duplicate Exports

**Impact:** Overlapping uploads may inflate event and threshold counts.

**Improvement:** Track export windows, source names, hashes, and ingestion history.

**Priority:** Medium

---

## GAP-006: Limited Identity and Asset Context

**Impact:** Hunts cannot compare user role, asset criticality, or expected administrative behaviour.

**Improvement:** Add identity, asset, ownership, and business-context enrichment.

**Priority:** Medium
