import React, { useState } from 'react';

const initialEvents = [
  { id: 1, title: "Community BBQ", date: "2025-06-15", location: "Town Park" },
  { id: 2, title: "Book Fair", date: "2025-06-20", location: "Library Hall" },
];
const initialBusinesses = [
  { id: 1, name: "Joe's Cafe", contact: "555-1234" },
  { id: 2, name: "Green Grocer", contact: "555-5678" },
];
const initialAnnouncements = [
  { id: 1, message: "Water outage in North Street, 9am-2pm tomorrow." },
  { id: 2, message: "Town Hall meeting rescheduled to June 18." },
];

function App() {
  const [tab, setTab] = useState("home");
  const [user, setUser] = useState(null);

  return (
    <div style={{ maxWidth: 500, margin: "0 auto", fontFamily: "sans-serif" }}>
      <h1>TownConnect</h1>
      {!user ? (
        <Register onRegister={setUser} />
      ) : (
        <>
          <NavBar tab={tab} setTab={setTab} />
          {tab === "home" && <Home user={user} />}
          {tab === "events" && <Events />}
          {tab === "businesses" && <Businesses />}
          {tab === "announcements" && <Announcements />}
          {tab === "profile" && <Profile user={user} setUser={setUser} />}
        </>
      )}
    </div>
  );
}

function NavBar({ tab, setTab }) {
  return (
    <nav style={{ marginBottom: 16 }}>
      <button onClick={() => setTab("home")} disabled={tab==="home"}>Home</button>
      <button onClick={() => setTab("events")} disabled={tab==="events"}>Events</button>
      <button onClick={() => setTab("businesses")} disabled={tab==="businesses"}>Businesses</button>
      <button onClick={() => setTab("announcements")} disabled={tab==="announcements"}>Announcements</button>
      <button onClick={() => setTab("profile")} disabled={tab==="profile"}>Profile</button>
    </nav>
  );
}

function Register({ onRegister }) {
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [phone, setPhone] = useState('');

  function handleSubmit(e) {
    e.preventDefault();
    if (!username) {
      alert("Username is required");
      return;
    }
    onRegister({ username, email, phone });
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Create Account</h2>
      <div>
        <label>
          Username*<br />
          <input value={username} onChange={e => setUsername(e.target.value)} required />
        </label>
      </div>
      <div>
        <label>
          Email<br />
          <input value={email} onChange={e => setEmail(e.target.value)} type="email" />
        </label>
      </div>
      <div>
        <label>
          Phone<br />
          <input value={phone} onChange={e => setPhone(e.target.value)} />
        </label>
      </div>
      <button type="submit">Register</button>
    </form>
  );
}

function Home({ user }) {
  return (
    <div>
      <h2>Welcome, {user.username}!</h2>
      <p>See what's happening in your town:</p>
      <Events short />
      <Businesses short />
      <Announcements short />
    </div>
  );
}

function Events({ short }) {
  const events = initialEvents.slice(0, short ? 1 : undefined);
  return (
    <div>
      <h3>Events</h3>
      <ul>
        {events.map(ev => (
          <li key={ev.id}>
            <b>{ev.title}</b> - {ev.date} @ {ev.location}
          </li>
        ))}
      </ul>
      {short && <small>More events in the 'Events' tab.</small>}
    </div>
  );
}

function Businesses({ short }) {
  const businesses = initialBusinesses.slice(0, short ? 1 : undefined);
  return (
    <div>
      <h3>Businesses</h3>
      <ul>
        {businesses.map(biz => (
          <li key={biz.id}>
            <b>{biz.name}</b> - Contact: {biz.contact}
          </li>
        ))}
      </ul>
      {short && <small>More businesses in the 'Businesses' tab.</small>}
    </div>
  );
}

function Announcements({ short }) {
  const announcements = initialAnnouncements.slice(0, short ? 1 : undefined);
  return (
    <div>
      <h3>Announcements</h3>
      <ul>
        {announcements.map(a => (
          <li key={a.id}>{a.message}</li>
        ))}
      </ul>
      {short && <small>More in the 'Announcements' tab.</small>}
    </div>
  );
}

function Profile({ user, setUser }) {
  const [username, setUsername] = useState(user.username);
  const [email, setEmail] = useState(user.email || '');
  const [phone, setPhone] = useState(user.phone || '');

  function handleSave() {
    setUser({ ...user, username, email, phone });
    alert("Profile updated!");
  }

  return (
    <div>
      <h2>Your Profile</h2>
      <div>
        <label>
          Username<br />
          <input value={username} onChange={e => setUsername(e.target.value)} />
        </label>
      </div>
      <div>
        <label>
          Email<br />
          <input value={email} onChange={e => setEmail(e.target.value)} />
        </label>
      </div>
      <div>
        <label>
          Phone<br />
          <input value={phone} onChange={e => setPhone(e.target.value)} />
        </label>
      </div>
      <button onClick={handleSave}>Save</button>
    </div>
  );
}

export default App;# EVENTS
an app designed to connect town locals to events, businesses, services and announcements
