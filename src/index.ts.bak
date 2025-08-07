import { ReplrodOptimizationPlugin } from '@listenrightmeow/replrod/plugin/OptimizationPlugin';
import { SocialMediaOptimizer } from './SocialMediaOptimizer.js';

const plugin: ReplrodOptimizationPlugin = {
  metadata: {
    name: '@listenrightmeow/replrod-plugin-social-media',
    version: '1.0.0',
    description: 'Social media optimization with OG images, Twitter cards, and favicons for replrod',
    author: '@listenrightmeow',
    expectedImprovement: 'Social media optimization with OG & Twitter cards'
  },
  optimization: new SocialMediaOptimizer(),
  
  async onLoad() {
    console.debug('Social media optimization plugin loaded');
  },
  
  async onUnload() {
    console.debug('Social media optimization plugin unloaded');
  }
};

export default plugin;
export { plugin };

// Export internal modules for testing
export { FaviconGenerator } from './FaviconGenerator.js';
export { OGImageGenerator } from './OGImageGenerator.js';
export { SocialMediaComponentGenerator } from './SocialMediaComponentGenerator.js';