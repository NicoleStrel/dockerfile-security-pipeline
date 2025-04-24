# Dockerfile Security Pipeline

A comprehensive SLSA Build Level 3 compliant pipeline that secures container builds by:

1. Building images using BuildKit (docker buildx)
2. Generating a Software Bill of Materials (SBOM) with Syft
3. Scanning for vulnerabilities using Trivy and Docker Scout
4. Signing the image with Cosign and generating attestations

## How It Works

This pipeline implements a comprehensive security approach for container images with specific jobs designed to enhance security at every stage:

1. **Build Job**:
   - Uses Docker BuildKit with enhanced security features
   - Leverages GitHub Actions for a reproducible build environment
   - Implements SLSA Build Level 3 compliant provenance generation
   - **Security Significance**: Ensures build integrity and provides verifiable build provenance, preventing supply chain attacks

2. **Scan-Vulnerabilities Job**:
   - Runs Trivy vulnerability scanner against the built image
   - Produces SARIF reports integrated with GitHub Security tab
   - Detects OS and library vulnerabilities with critical and high severity
   - **Security Significance**: Identifies known vulnerabilities before deployment, reducing the attack surface

3. **Scan-with-Docker-Scout Job**:
   - Provides a complementary vulnerability scan using Docker Scout
   - Offers another perspective on security issues that might be missed by Trivy
   - Also integrates with GitHub Security tab
   - **Security Significance**: Multiple scanning engines increase the likelihood of catching all vulnerabilities

4. **Sign-and-Attest Job**:
   - Signs container images using Cosign with keyless signing (OIDC-based)
   - Creates and verifies in-toto attestations for provenance and SBOM
   - **Security Significance**: Enables consumers to verify image authenticity and integrity, preventing tampering

## Usage

1. Add this GitHub Actions workflow to your repository
2. Configure your Dockerfile in the right location
3. Create a pull request to trigger the pipeline
4. View security results in the GitHub Security tab and workflow artifacts

## Example

The repository includes an example pull request with a Dockerfile based on `node:16` with vulnerable dependencies to demonstrate the pipeline's effectiveness:

- The base image `node:16` contains known vulnerabilities
- The `package.json` includes outdated dependencies with known CVEs (lodash 4.17.15)
- Running the pipeline will detect and report these issues

Security scan results appear in pull requests via GitHub Security tab and within the pull request itself, providing immediate feedback before merging.

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
