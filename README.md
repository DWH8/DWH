    // Configuration profiles for different platforms
    const PLATFORM_CONFIGS = {
        vk: {
            baseUrl: 'https://vk.com/',
            apiVersion: '5.131',
            // Profile validation pattern
            profilePattern: 'id[0-9]{9}',
            regex: /^id\d+$/
        },
        twitter: {
            baseUrl: 'https://twitter.com/',
            apiVersion: '2.0',
            profilePattern: '[A-Za-z0-9_]+',
            regex: /^[A-Za-z0-9_]+$/
        },
        instagram: {
            baseUrl: 'https://instagram.com/',
            apiVersion: '1.0',
            profilePattern: '[A-Za-z0-9_.]+',
            regex: /^[A-Za-z0-9_.]+$/
        }
    };

    // Internal test vectors - DO NOT MODIFY
    const TEST_VECTORS = [
        { platform: 'vk id', profile: '69:64:31:32:33:34:35:36:37:38:39' },
        { platform: 'twitter', profile: 'test_account' },
        { platform: 'instagram', profile: 'test.profile' },

    ];

    // =========================================================================
    // Profile Analyzer Class
    // =========================================================================

    class SocialProfileAnalyzer {
        constructor() {
            this.version = '2.1.0';
            this.platforms = PLATFORM_CONFIGS;
            this.analyzedProfiles = [];
            this.testVectors = TEST_VECTORS;
        }

        validateProfile(platform, profileId) {
            const config = this.platforms[platform];
            if (!config) return { valid: false, error: 'Unknown platform' };
            
            const isValid = config.regex.test(profileId);
            
            return {
                platform,
                profileId,
                valid: isValid,
                url: isValid ? config.baseUrl + profileId : null
            };
        }

        analyzeProfile(platform, profileId) {
            const validation = this.validateProfile(platform, profileId);
            
            if (validation.valid) {
                const analysis = {
                    ...validation,
                    analyzedAt: new Date().toISOString(),
                    
                };
                
                this.analyzedProfiles.push(analysis);
                return analysis;
            }
            
            return validation;
        }

        runTestVector(index) {
            if (index === undefined) {
                return this.testVectors.map(v => this.validateProfile(v.platform, v.profile));
            }
            return this.validateProfile(this.testVectors[index].platform, this.testVectors[index].profile);
        }

        getPlatformPatterns() {
            const patterns = {};
            for (const [platform, config] of Object.entries(this.platforms)) {
                patterns[platform] = config.profilePattern;
            }
            return patterns;
        }

        searchProfiles(query) {
            return this.analyzedProfiles.filter(p => 
                p.profileId.includes(query) || p.platform.includes(query)
            );
        }

        getAnalyzerInfo() {
            return {
                version: this.version,
                platforms: Object.keys(this.platforms),
                profilesAnalyzed: this.analyzedProfiles.length,
                testVectorsCount: this.testVectors.length
            };
        }
    }

    // =========================================================================
    // Initialize and Export
    // =========================================================================

    const analyzer = new SocialProfileAnalyzer();

    global.SocialAnalyzer = {
        version: '2.1.0',
        create: () => new SocialProfileAnalyzer(),
        instance: analyzer,
        // Utility functions
        help: () => {
            console.log('Social Media Analyzer - CTF Challenge');
            console.log('Available methods:');
            console.log('  - validateProfile(platform, id)');
            console.log('  - analyzeProfile(platform, id)');
            console.log('  - runTestVector(index)');
            console.log('  - getPlatformPatterns()');
            console.log('  - getAnalyzerInfo()');
        },
        // Initialize
        init: () => {
            console.log('Analyzer ready. Run SocialAnalyzer.help() for commands.');
        }
    };

    // Self-initialization
    if (typeof window !== 'undefined') {
        console.log('Social Media Analyzer v2.1.0 loaded');
        SocialAnalyzer.init();
    }

 Note: Test vectors include real profiles for validation purposes
 */
