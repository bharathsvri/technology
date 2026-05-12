# Spring Boot: SBOM and Supply Chain Transparency

A **Software Bill of Materials (SBOM)** inventories the libraries embedded in your deliverable. Regulators and enterprise buyers increasingly expect SBOMs for **vulnerability response** (e.g., knowing if Log4j coordinates appear in a shipped JAR).

## Spring Boot Support (Conceptual)
Recent Spring Boot generations expose SBOM-related **Actuator** endpoints and/or build plugin integration—check your **exact Boot version** in the reference manual (feature set expanded around the 3.2–3.4 timeframe).

Typical goals:
- Generate **CycloneDX** or **SPDX** JSON during `mvn package` / `./gradlew build`.
- Publish or archive SBOMs next to release artifacts in CI.

## Build-Time Generation (Pattern)
### Maven (CycloneDX plugin)
Add the official **CycloneDX Maven plugin** to your `pom.xml` so SBOM JSON is emitted into `target/`.

### Gradle (CycloneDX plugin)
Apply the **CycloneDX Gradle plugin** to emit JSON under `build/reports/bom.json` (exact path depends on configuration).

## Runtime vs Build-Time SBOM
- **Build-time SBOM**: complete dependency graph including transitive libraries—best for **security scanning** in CI.
- **Runtime SBOM** (when exposed via Actuator): reflects what is **actually loaded**—useful for drift detection between build graph and packaged layers.

## Operational Practices
- Store SBOMs **immutably per release** (artifact repository alongside the JAR).
- Feed SBOMs into **Dependency-Track**, **Grype**, **Trivy SBOM** mode, or vendor scanners.
- Treat SBOM as **sensitive metadata** if it reveals internal library choices.

## Related Topics
- Java: `94_CI_CD_Java.md`, `65_Code_Quality_Tools.md`
- `14-Spring-Boot-Actuator.md`
