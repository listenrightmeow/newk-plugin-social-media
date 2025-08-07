import * as fs from 'fs/promises';
import * as path from 'path';

export class SocialMediaComponentGenerator {
  async generate(projectPath: string): Promise<void> {
    const componentDir = path.join(projectPath, 'client/src/components/SocialMedia');
    await fs.mkdir(componentDir, { recursive: true });
    
    await this.createShareComponent(componentDir);
    await this.createMetaComponent(componentDir);
  }

  private async createShareComponent(dir: string): Promise<void> {
    const content = `import React from 'react';

interface ShareButtonsProps {
  url?: string;
  title?: string;
  description?: string;
}

const ShareButtons: React.FC<ShareButtonsProps> = ({
  url = window.location.href,
  title = document.title,
  description = ''
}) => {
  const encodedUrl = encodeURIComponent(url);
  const encodedTitle = encodeURIComponent(title);
  const encodedDesc = encodeURIComponent(description);

  const shareLinks = {
    twitter: \`https://twitter.com/intent/tweet?url=\${encodedUrl}&text=\${encodedTitle}\`,
    facebook: \`https://www.facebook.com/sharer/sharer.php?u=\${encodedUrl}\`,
    linkedin: \`https://www.linkedin.com/sharing/share-offsite/?url=\${encodedUrl}\`,
    reddit: \`https://reddit.com/submit?url=\${encodedUrl}&title=\${encodedTitle}\`,
    email: \`mailto:?subject=\${encodedTitle}&body=\${encodedDesc}%0A%0A\${encodedUrl}\`
  };

  const openShareWindow = (url: string) => {
    window.open(url, '_blank', 'width=600,height=400');
  };

  return (
    <div className="share-buttons">
      <button onClick={() => openShareWindow(shareLinks.twitter)} aria-label="Share on Twitter">
        Twitter
      </button>
      <button onClick={() => openShareWindow(shareLinks.facebook)} aria-label="Share on Facebook">
        Facebook
      </button>
      <button onClick={() => openShareWindow(shareLinks.linkedin)} aria-label="Share on LinkedIn">
        LinkedIn
      </button>
      <button onClick={() => openShareWindow(shareLinks.reddit)} aria-label="Share on Reddit">
        Reddit
      </button>
      <a href={shareLinks.email} aria-label="Share via Email">
        Email
      </a>
    </div>
  );
};

export default ShareButtons;`;
    
    await fs.writeFile(path.join(dir, 'ShareButtons.tsx'), content);
  }

  private async createMetaComponent(dir: string): Promise<void> {
    const content = `import React from 'react';
import { Helmet } from 'react-helmet-async';

interface SocialMetaProps {
  title?: string;
  description?: string;
  image?: string;
  url?: string;
  twitterUsername?: string;
}

const SocialMeta: React.FC<SocialMetaProps> = ({
  title = 'Your Site Title',
  description = 'Your site description',
  image = '/og-image.jpg',
  url = window.location.href,
  twitterUsername
}) => {
  return (
    <Helmet>
      {/* Open Graph */}
      <meta property="og:title" content={title} />
      <meta property="og:description" content={description} />
      <meta property="og:image" content={image} />
      <meta property="og:url" content={url} />
      <meta property="og:type" content="website" />
      
      {/* Twitter Card */}
      <meta name="twitter:card" content="summary_large_image" />
      <meta name="twitter:title" content={title} />
      <meta name="twitter:description" content={description} />
      <meta name="twitter:image" content={image} />
      {twitterUsername && (
        <meta name="twitter:creator" content={twitterUsername} />
      )}
    </Helmet>
  );
};

export default SocialMeta;`;
    
    await fs.writeFile(path.join(dir, 'SocialMeta.tsx'), content);
  }
}