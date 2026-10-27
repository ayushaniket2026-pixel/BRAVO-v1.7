The full site source is in artifacts/bravo-ai-mastery/src/App.tsx; the styling is in artifacts/bravo-ai-mastery/src/index.css. The /learn page renders the video section with <VideoLibrary />.

Here’s the video catalog and player component:

type VideoResource = {
  id: string;
  title: string;
  creator: string;
  track: "AI" | "Python" | "Web development";
  format: "Course" | "Tutorial";
  duration: string;
  level: string;
  videoId: string;
  playlistId?: string;
  description: string;
};
const videoResources: VideoResource[] = [
  {
    id: "ai-course",
    title: "Artificial Intelligence for Everyone",
    creator: "Paul McWhorter",
    track: "AI",
    format: "Course",
    duration: "50-part playlist",
    level: "Beginner",
    videoId: "gD_HWj_hvbo",
    playlistId: "PLGs0VKk2DiYyXlbJVaE8y1qr24YldYNDm",
    description:
      "A step-by-step introduction to AI fundamentals, starting with what artificial intelligence is.",
  },
  {
    id: "ai-tutorial",
    title: "AI Basics for Beginners",
    creator: "codebasics",
    track: "AI",
    format: "Tutorial",
    duration: "56 min",
    level: "Beginner",
    videoId: "VGFpV3Qj4as",
    description:
      "A clear overview of AI, machine learning, deep learning, and generative AI.",
  },
  {
    id: "python-course",
    title: "Python for Beginners — Full Course",
    creator: "freeCodeCamp.org",
    track: "Python",
    format: "Course",
    duration: "4 hr 40 min",
    level: "Beginner",
    videoId: "eWRfhZUzrAc",
    description:
      "Learn Python fundamentals and follow along with two beginner-friendly projects.",
  },
  {
    id: "python-tutorial",
    title: "Python Crash Course for Beginners",
    creator: "Traversy Media",
    track: "Python",
    format: "Tutorial",
    duration: "1 hr 35 min",
    level: "Beginner",
    videoId: "JJmcL1N2KQs",
    description:
      "A practical crash course covering Python basics, data structures, functions, and more.",
  },
  {
    id: "html-course",
    title: "Learn HTML — Full Tutorial for Beginners",
    creator: "freeCodeCamp.org",
    track: "Web development",
    format: "Course",
    duration: "4 hr 7 min",
    level: "Beginner",
    videoId: "kUMe1FH4CHE",
    description:
      "Build a foundation in HTML, from page structure and links to forms and a small project.",
  },
  {
    id: "css-course",
    title: "CSS Tutorial — Zero to Hero",
    creator: "freeCodeCamp.org",
    track: "Web development",
    format: "Course",
    duration: "6 hr 18 min",
    level: "Beginner",
    videoId: "1Rs2ND1ryYc",
    description:
      "Learn the essentials of styling, layout, and responsive design with CSS.",
  },
  {
    id: "javascript-course",
    title: "Learn JavaScript — Full Course",
    creator: "freeCodeCamp.org",
    track: "Web development",
    format: "Course",
    duration: "3 hr 26 min",
    level: "Beginner",
    videoId: "PkZNo7MFNFg",
    description:
      "A beginner course on JavaScript fundamentals, functions, objects, and programming logic.",
  },
  {
    id: "website-tutorial",
    title: "Build a Simple Website with HTML, CSS & JavaScript",
    creator: "freeCodeCamp.org",
    track: "Web development",
    format: "Tutorial",
    duration: "7-hour course",
    level: "Beginner",
    videoId: "krfUjg0S2uI",
    description:
      "Follow a project-based tutorial and put HTML, CSS, and JavaScript together in a website.",
  },
];
function VideoLibrary() {
  const [trackFilter, setTrackFilter] =
    useState<"All" | VideoResource["track"]>("All");
  const [formatFilter, setFormatFilter] =
    useState<"All" | VideoResource["format"]>("All");
  const [selectedId, setSelectedId] = useState("ai-course");
  const visibleVideos = videoResources.filter(
    (video) =>
      (trackFilter === "All" || video.track === trackFilter) &&
      (formatFilter === "All" || video.format === formatFilter),
  );
  const selectedVideo =
    visibleVideos.find((video) => video.id === selectedId) ??
    visibleVideos[0] ??
    videoResources[0];
  const embedUrl = selectedVideo.playlistId
    ? `https://www.youtube-nocookie.com/embed/${selectedVideo.videoId}?list=${selectedVideo.playlistId}&rel=0`
    : `https://www.youtube-nocookie.com/embed/${selectedVideo.videoId}?rel=0`;
  const youtubeUrl = selectedVideo.playlistId
    ? `https://www.youtube.com/playlist?list=${selectedVideo.playlistId}`
    : `https://www.youtube.com/watch?v=${selectedVideo.videoId}`;
  return (
    <section className="video-library" aria-label="YouTube courses and tutorials">
      <div className="video-library-heading">
        <div>
          <div className="eyebrow">YOUTUBE LEARNING</div>
          <h2>Courses &amp; tutorials</h2>
          <p>
            Watch a full course or pick a focused tutorial. Videos play here
            and open on YouTube if you prefer.
          </p>
        </div>
        <span className="video-source-note">
          Videos by their original creators
        </span>
      </div>
      <div className="video-filters" aria-label="Filter videos">
        <div className="video-filter-group" aria-label="Filter by subject">
          {(["All", "AI", "Python", "Web development"] as const).map(
            (track) => (
              <button
                key={track}
                className={`video-filter ${trackFilter === track ? "active" : ""}`}
                onClick={() => setTrackFilter(track)}
              >
                {track}
              </button>
            ),
          )}
        </div>
        <div className="video-filter-group" aria-label="Filter by video type">
          {(["All", "Course", "Tutorial"] as const).map((format) => (
            <button
              key={format}
              className={`video-filter ${formatFilter === format ? "active" : ""}`}
              onClick={() => setFormatFilter(format)}
            >
              {format === "All" ? "All videos" : `${format}s`}
            </button>
          ))}
        </div>
      </div>
      <div className="video-library-layout">
        <article className="video-feature">
          <div className="video-frame">
            <iframe
              src={embedUrl}
              title={`${selectedVideo.title} by ${selectedVideo.creator}`}
              loading="lazy"
              referrerPolicy="strict-origin-when-cross-origin"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowFullScreen
            />
          </div>
          <div className="video-feature-copy">
            <span className="tag">
              {selectedVideo.track} · {selectedVideo.format}
            </span>
            <h3>{selectedVideo.title}</h3>
            <p>{selectedVideo.description}</p>
            <span>By {selectedVideo.creator} · {selectedVideo.level}</span>
            {" "}
            <a href={youtubeUrl} target="_blank" rel="noreferrer">
              Watch on YouTube
            </a>
          </div>
        </article>
        <div className="video-list" aria-label="Available videos">
          {visibleVideos.map((video) => (
            <button
              key={video.id}
              className={`video-list-item ${
                selectedVideo.id === video.id ? "selected" : ""
              }`}
              onClick={() => setSelectedId(video.id)}
            >
              <span className="video-list-copy">
                <strong>{video.title}</strong>
                <small>{video.creator} · {video.duration}</small>
                <span>{video.track} · {video.format}</span>
              </span>
            </button>
          ))}
        </div>
      </div>
    </section>
  );
}
