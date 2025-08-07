import { 
  OptimizationPlugin, 
  OptimizationContext, 
  OptimizationResult 
} from '@listenrightmeow/replrod/plugin/OptimizationPlugin';
import { FaviconGenerator } from './FaviconGenerator.js';
import { OGImageGenerator } from './OGImageGenerator.js';
import { SocialMediaComponentGenerator } from './SocialMediaComponentGenerator.js';

export class SocialMediaOptimizer implements OptimizationPlugin {
  name = 'social-media';
  description = 'Generate social media assets';
  category = 'seo' as const;
  priority = 3;

  private favicon = new FaviconGenerator();
  private ogImage = new OGImageGenerator();
  private components = new SocialMediaComponentGenerator();

  async isApplicable(_context: OptimizationContext): Promise<boolean> {
    return true;
  }

  async optimize(context: OptimizationContext): Promise<OptimizationResult> {
    try {
      const start = Date.now();
      const fileCount = await this.generateAssets(context);
      return this.createResult(fileCount, start);
    } catch (error) {
      return this.createError(error);
    }
  }

  async validate(context: OptimizationContext): Promise<boolean> {
    try {
      await this.checkAssets(context);
      return true;
    } catch {
      return false;
    }
  }

  private async generateAssets(
    context: OptimizationContext
  ): Promise<number> {
    const faviconCount = await this.favicon.generate(context.projectPath);
    await this.ogImage.generate(context.projectPath, context.config.socialMedia);
    await this.components.generate(context.projectPath);
    
    return faviconCount + 3; // favicons + 2 OG images + 2 components
  }

  private async checkAssets(context: OptimizationContext): Promise<void> {
    const path = await import('path');
    const fs = await import('fs/promises');
    
    const publicDir = path.join(context.projectPath, 'client', 'public');
    const ogPath = path.join(publicDir, 'og-image.jpg');
    const faviconPath = path.join(publicDir, 'favicon.ico');
    
    await fs.access(ogPath);
    await fs.access(faviconPath);
  }

  private createResult(count: number, start: number): OptimizationResult {
    return {
      success: true,
      message: `Generated ${count} social media assets`,
      metrics: {
        improvement: 'Complete social media optimization',
        timeBefore: start,
        timeAfter: Date.now()
      }
    };
  }

  private createError(error: unknown): OptimizationResult {
    const message = error instanceof Error ? error.message : String(error);
    return {
      success: false,
      error: `Social media optimization failed: ${message}`
    };
  }
}