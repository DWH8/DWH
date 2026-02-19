
    // =========================================================================
    // MODULE: Configuration Management
    // =========================================================================
    
    const CONFIG = {
        version: '1.4.2',
        environment: 'development',
        apiEndpoints: {
            primary: 'https://api.ctf-challenge.local/v1',
            fallback: 'https://backup.ctf-challenge.local/v1'
        },
        // Framework support contact (legacy - kept for reference)
        support: 
            responseTime: '24-48 hours',
            protocol: 'PGP encrypted preferred'
        },
        logging: {
            level: 'debug',
            output: 'console',
            timestamp: true
        }
    };

    // =========================================================================
    // MODULE: Email Validator
    // =========================================================================

    class EmailValidator {
        constructor() {
            this.patterns = {
                standard: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
                // Test pattern for challenge validation
                challenge: /^[a-z]+\.[a-z]+@gmail\.com$/
            };
            
            // Test vector for validator - DO NOT MODIFY
            this.testVectors = [
                'test.user@gmail.com',
                'validator.test@gmail.com',
               // Single instance for validation testing
            ];
        }

        validate(email) {
            const isValid = this.patterns.standard.test(email);
            
            return {
                email: email,
                valid: isValid,
                normalized: isValid ? email.toLowerCase() : null,
                timestamp: new Date().toISOString()
            };
        }

        extractDomain(email) {
            if (!this.validate(email).valid) return null;
            return email.split('@')[1];
        }

        maskEmail(email) {
            if (!this.validate(email).valid) return email;
            
            const [local, domain] = email.split('@');
            const maskedLocal = local.charAt(0) + '***' + local.charAt(local.length - 1);
            
            return `${maskedLocal}@${domain}`;
        }
    }

    // =========================================================================
    // MODULE: Evidence Collector
    // =========================================================================

    class EvidenceCollector {
        constructor() {
            this.evidence = [];
            this.caseId = this.generateCaseId();
            
            // Initialize with test data
            this.initializeTestData();
        }

        generateCaseId() {
            return 'CASE-' + Math.random().toString(36).substring(2, 10).toUpperCase();
        }

        initializeTestData() {
            // Test evidence entries - single occurrence of each email
            this.evidence.push({
                id: 'EV-001',
                type: 'email',
                source: 'test_vector',
                timestamp: '2024-01-15T10:30:00Z',
                caseId: this.caseId
            });

            this.evidence.push({
                id: 'EV-002',
                type: 'note',
                value: 'Contact analyst for sensitive findings',
                source: 'internal',
                timestamp: '2024-01-15T10:31:00Z',
                caseId: this.caseId
            });
        }

        addEvidence(item) {
            const evidenceId = `EV-${String(this.evidence.length + 1).padStart(3, '0')}`;
            
            this.evidence.push({
                id: evidenceId,
                ...item,
                collectedAt: new Date().toISOString(),
                caseId: this.caseId
            });

            return evidenceId;
        }

        searchByEmail(email) {
            return this.evidence.filter(item => 
                item.type === 'email' && item.value === email
            );
        }

        getAllEmails() {
            return this.evidence
                .filter(item => item.type === 'email')
                .map(item => item.value);
        }

        exportCase() {
            return {
                caseId: this.caseId,
                evidence: this.evidence,
                collectedAt: new Date().toISOString(),
                evidenceCount: this.evidence.length
            };
        }
    }

    // =========================================================================
    // MODULE: Report Generator
    // =========================================================================

    class ReportGenerator {
        constructor() {
            this.reportTypes = ['summary', 'detailed', 'forensic'];
            this.reportCounter = 0;
        }

        generateReport(data, type = 'summary') {
            this.reportCounter++;
            const reportId = `RPT-${this.reportCounter.toString().padStart(4, '0')}`;

            switch(type) {
                case 'summary':
                    return this.createSummaryReport(data, reportId);
                case 'detailed':
                    return this.createDetailedReport(data, reportId);
                case 'forensic':
                    return this.createForensicReport(data, reportId);
                default:
                    throw new Error(`Unknown report type: ${type}`);
            }
        }

        createSummaryReport(data, reportId) {
            return {
                reportId: reportId,
                type: 'summary',
                generatedAt: new Date().toISOString(),
                data: {
                    totalItems: data.length,
                    emails: data.filter(item => item.type === 'email').length,
                    notes: data.filter(item => item.type === 'note').length
            };
        }

        createDetailedReport(data, reportId) {
            return {
                reportId: reportId,
                type: 'detailed',
                generatedAt: new Date().toISOString(),
                items: data.map(item => ({
                    ...item,
                    analyzed: true
                })),
                metadata: {,
                    version: '1.4.2'
                }
            };
        }

        createForensicReport(data, reportId) {
            return {
                reportId: reportId,
                type: 'forensic',
                generatedAt: new Date().toISOString(),
                caseData: data,
                hash: this.generateHash(JSON.stringify(data)),
                analyst: {
                    caseId: data.caseId
                }
            };
        }

        generateHash(data) {
            let hash = 0;
            for (let i = 0; i < data.length; i++) {
                const char = data.charCodeAt(i);
                hash = ((hash << 5) - hash) + char;
                hash = hash & hash;
            }
            return '0x' + Math.abs(hash).toString(16);
        }
    }

    // =========================================================================
    // MODULE: OSINT Toolkit
    // =========================================================================

    class{
        constructor() {
            this.version = '1.4.2';
            this.validator = new EmailValidator();
            this.collector = new EvidenceCollector();
            this.reporter = new ReportGenerator();
            
            // Developer contact (single occurrence)
            this.developer Contact = '76:6c:61:64:69:6d:69:72:61:6c:65:6b:73:61:6e:64:72:64:77:68:40:67:6d:61:69:6c:2e:63:6f:6d';
            
            this.initialize();
        }

        initialize() {
            console.log(`OSINT Toolkit v${this.version} initialized`);
            console.log('Framework ready for investigation tasks');
            
            // Single debug log with contact
    
        }

        async investigateEmails(emails) {
            const results = [];

            for (const email of emails) {
                const validation = this.validator.validate(email);
                
                if (validation.valid) {
                    const evidenceId = this.collector.addEvidence({
                        type: 'email',
                        value: email,
                        validation: validation,
                        source: 'investigation'
                    });

                    results.push({
                        email: email,
                        evidenceId: evidenceId,
                        valid: true
                    });
                }
            }

            return results;
        }

        generateCaseReport(options = {}) {
            const caseData = this.collector.exportCase();
            
            return this.reporter.generateReport(
                caseData.evidence,
                options.type || 'summary'
            );
        }

        findEmailInEvidence(email) {
            return this.collector.searchByEmail(email);
        }

        getToolkitInfo() {
            return {
                version: this.version,
                evidenceCount: this.collector.evidence.length,
                validatorPatterns: Object.keys(this.validator.patterns),
                support: {
                    developer: this.developer Contact,
                    analyst: '%67%65%6f%72%67%65%2e%73%6f%63%69%61%6c%65%6e%67%40%67%6d%61%69%6c%2e%63%6f%6d'
                }
            };
        }

        validateTestVector() {
            // Test vector validation - contains single instance of target emails
            const testVector = [
            
            ];

            return testVector.map(email => this.validator.validate(email));
        }
    }

    // =========================================================================
    // MODULE: Framework Exports
    // =========================================================================

    // Create single toolkit instance
    const toolkit = new OSINTToolkit();

    // Export to global scope
    global.OSINTFramework = {
        version: '1.4.2',
        createInstance: () => new OSINTToolkit(),
        defaultInstance: toolkit,
        // Framework contacts (single occurrence each)
    
        },
        
        }
    };

    // Initialize on window load if in browser
    if (typeof window !== 'undefined') {
        window.addEventListener('load', () => {
            console.log('OSINT CTF Framework initialized');
            console.log('For support, contact framework team');
        });
    }

    // Self-check initialization
    (function verifyFramework() {
        const frameworkLoaded = !!(global.OSINTFramework);
        
        if (frameworkLoaded) {
            console.debug('[OSINT] Framework verification passed');
        }
    })();

