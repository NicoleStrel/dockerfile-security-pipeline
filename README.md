# Dockerfile Security Pipeline

A comprehensive SLSA Build Level 3 compliant pipeline that secures container builds by:

1. Building images using BuildKit (docker buildx), and generating SBOM
2. Scanning for CVEs using Trivy and Docker Scout
3. Signing the image with Cosign and generating attestations

## How It Works

This pipeline implements a comprehensive security approach for container images with specific jobs designed to enhance security at every stage:

1. **Build Job**:
   - Uses Docker BuildKit with enhanced security features
   - Leverages GitHub Actions for a reproducible build environment
   - Implements SLSA Build Level 3 compliant provenance generation
   - SBOM generation
   - **Security Significance**: Ensures build integrity and provides verifiable build provenance, preventing supply chain attacks

2. **Scan-with-Trivy Job**:
   - Runs Trivy vulnerability scanner against the built image
   - Produces SARIF reports integrated with GitHub Security tab
   - Fails if CVEs are detected and reports the findings in the PR comment.
   - **Security Significance**: Identifies known vulnerabilities before deployment, reducing the attack surface

3. **Scan-with-Docker-Scout Job**:
   - Provides a complementary vulnerability scan using Docker Scout
   - Offers another perspective on security issues that might be missed by Trivy
   - Also integrates with GitHub Security tab, detects CVEs, gives recommendations + comparisons
   - **Security Significance**: Multiple scanning engines increase the likelihood of catching all vulnerabilities

4. **Sign-and-Attest Job**:
   - Signs container images using Cosign with keyless signing (OIDC-based)
   - Creates and verifies in-toto attestations for provenance and SBOM
   - **Security Significance**: Enables consumers to verify image authenticity and integrity, preventing tampering

## Usage

1. Add this GitHub Actions workflow to your repository
2. Configure your Dockerfile in the right location
3. Add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` to your repo's action secrets.
4. Create a pull request to trigger the pipeline
5. View security results in the GitHub Security tab and pull request comments

## Running the Pipeline Locally

You can trigger the pipeline remotely using GitHub CLI, which allows you to run GitHub Actions workflows directly from your terminal:

1. Install GitHub CLI
2. Authenticate with GitHub
   ```bash
   gh auth login
   ```
3. Run the pipeline:
   ```bash
   # Run the entire pipeline (recommended)
   gh workflow run security-pipeline.yml
   
   # Run individual workflows
   gh workflow run build.yml
   
   # Monitor the workflow progress
   gh run list --workflow=security-pipeline.yml
   ```

## Vulnerable Example

pull request: https://github.com/NicoleStrel/dockerfile-security-pipeline/pull/4 

The repository includes an example pull request with a vulnerable `node:14.17.0` base image to demonstrate the pipeline's effectiveness.

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
