import * as fs from 'fs/promises';
import * as path from 'path';
import sharp from 'sharp';

export class OGImageGenerator {
  private readonly width = 1200;
  private readonly height = 630;

  async generate(projectPath: string, config?: any): Promise<void> {
    const publicDir = path.join(projectPath, 'client', 'public');
    await fs.mkdir(publicDir, { recursive: true });
    
    const svg = this.createOGImageSvg(config);
    const buffer = await this.svgToBuffer(svg);
    
    await this.saveImage(buffer, publicDir);
  }

  private createOGImageSvg(config?: any): string {
    const title = config?.title || 'Your Amazing App';
    const subtitle = config?.subtitle || 'Build something incredible';
    const bgColor = config?.bgColor || '#3B82F6';
    const textColor = config?.textColor || '#ffffff';
    
    return `<svg width="${this.width}" height="${this.height}" xmlns="http://www.w3.org/2000/svg">
  <rect width="${this.width}" height="${this.height}" fill="${bgColor}"/>
  
  <!-- Background pattern -->
  <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
    <path d="M 40 0 L 0 0 0 40" fill="none" stroke="${textColor}" stroke-width="0.5" opacity="0.1"/>
  </pattern>
  <rect width="${this.width}" height="${this.height}" fill="url(#grid)"/>
  
  <!-- Content -->
  <text x="${this.width/2}" y="${this.height/2 - 40}" 
        font-family="system-ui, -apple-system, sans-serif" 
        font-size="72" font-weight="bold" fill="${textColor}" 
        text-anchor="middle">${this.escapeXml(title)}</text>
  
  <text x="${this.width/2}" y="${this.height/2 + 40}" 
        font-family="system-ui, -apple-system, sans-serif" 
        font-size="36" fill="${textColor}" opacity="0.9" 
        text-anchor="middle">${this.escapeXml(subtitle)}</text>
  
  <!-- Logo placeholder -->
  <circle cx="${this.width/2}" cy="${this.height - 100}" r="40" 
          fill="${textColor}" opacity="0.2"/>
</svg>`;
  }

  private escapeXml(text: string): string {
    return text
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&apos;');
  }

  private async svgToBuffer(svg: string): Promise<Buffer> {
    return sharp(Buffer.from(svg))
      .jpeg({ quality: 90 })
      .toBuffer();
  }

  private async saveImage(buffer: Buffer, publicDir: string): Promise<void> {
    await fs.writeFile(
      path.join(publicDir, 'og-image.jpg'),
      buffer
    );
    
    // Also save as PNG for better quality
    const pngBuffer = await sharp(buffer)
      .png()
      .toBuffer();
      
    await fs.writeFile(
      path.join(publicDir, 'og-image.png'),
      pngBuffer
    );
  }
}