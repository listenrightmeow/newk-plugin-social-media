import * as fs from 'fs/promises';
import * as path from 'path';
import sharp from 'sharp';

export class FaviconGenerator {
  private readonly sizes = [
    { size: 16, name: 'favicon-16x16.png' },
    { size: 32, name: 'favicon-32x32.png' },
    { size: 180, name: 'apple-touch-icon.png' },
    { size: 192, name: 'android-chrome-192x192.png' },
    { size: 512, name: 'android-chrome-512x512.png' }
  ];

  async generate(projectPath: string): Promise<number> {
    const publicDir = path.join(projectPath, 'client', 'public');
    await fs.mkdir(publicDir, { recursive: true });
    
    const svgContent = this.createSvgIcon();
    await fs.writeFile(path.join(publicDir, 'favicon.svg'), svgContent);
    
    const buffer = await this.svgToBuffer(svgContent);
    await this.generateSizes(buffer, publicDir);
    await this.createWebManifest(publicDir);
    
    return this.sizes.length + 3; // sizes + svg + ico + manifest
  }

  private createSvgIcon(): string {
    return `<svg width="32" height="32" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
  <rect width="32" height="32" fill="#3B82F6" rx="6"/>
  <circle cx="16" cy="16" r="8" fill="white"/>
  <circle cx="16" cy="16" r="4" fill="#3B82F6"/>
</svg>`;
  }

  private async svgToBuffer(svg: string): Promise<Buffer> {
    return sharp(Buffer.from(svg))
      .resize(512, 512)
      .png()
      .toBuffer();
  }

  private async generateSizes(buffer: Buffer, publicDir: string): Promise<void> {
    // Generate favicon.ico
    await sharp(buffer)
      .resize(32, 32)
      .toFile(path.join(publicDir, 'favicon.ico'));
    
    // Generate other sizes
    for (const { size, name } of this.sizes) {
      await sharp(buffer)
        .resize(size, size)
        .png()
        .toFile(path.join(publicDir, name));
    }
  }

  private async createWebManifest(publicDir: string): Promise<void> {
    const manifest = {
      name: 'Your App Name',
      short_name: 'YourApp',
      icons: [
        {
          src: '/android-chrome-192x192.png',
          sizes: '192x192',
          type: 'image/png'
        },
        {
          src: '/android-chrome-512x512.png',
          sizes: '512x512',
          type: 'image/png'
        }
      ],
      theme_color: '#3B82F6',
      background_color: '#ffffff',
      display: 'standalone'
    };
    
    await fs.writeFile(
      path.join(publicDir, 'manifest.json'),
      JSON.stringify(manifest, null, 2)
    );
  }
}