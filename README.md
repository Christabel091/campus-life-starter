import { Link } from "react-router-dom";
import "../styles/pages.css";

const Welcome = () => {
  return (
    <div className="overlay">
      <div className="welcome-page spotOn">
        <h1 className="logo-title">SpotOn</h1>
        <p className="tagline">Revolutionizing campus parking, one spot at a time.</p>
        <p className="description">
          Discover, reserve, and park smarter. Say goodbye to circling lots.
        </p>

        <ul className="features">
          <li>🔍 Search for nearby parking in real-time</li>
          <li>📍 Navigate directly to available spots</li>
          <li>🛡️ Reserve with confidence — no guesswork</li>
        </ul>

        <div className="button-group">
          <Link to="/signup">
            <button className="btn-start">Get Started</button>
          </Link>
          <Link to="/login">
            <button className="btn-start secondary">Log In</button>
          </Link>
        </div>
      </div>
    </div>
  );
};

export default Welcome;





.overlay {
  position: fixed;
  inset: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  z-index: 100;
  background: linear-gradient(45deg, #1c2e46 0%, #7c94b4 100%);
}

.welcome-page {
  text-align: center;
  max-width: 600px;
  margin: 2rem auto;
  background: #ffffff;
  border-radius: 16px;
  padding: 3rem 2rem;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
  animation: drop 0.6s ease-out forwards;
}

@keyframes drop {
  0% {
    opacity: 0;
    transform: translateY(-80px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.logo-title {
  font-size: 3.5rem;
  color: #1c2e46;
  margin-bottom: 0.5rem;
}

.tagline {
  font-size: 1.4rem;
  color: #444;
  margin-bottom: 1rem;
  font-weight: 600;
}

.description {
  font-size: 1.1rem;
  color: #666;
  margin-bottom: 2rem;
}

.features {
  list-style: none;
  padding: 0;
  margin-bottom: 2rem;
  color: #555;
  font-size: 1rem;
  line-height: 1.6;
}

.button-group {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.btn-start {
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(45deg, #1c2e46 0%, #7c94b4 100%);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.btn-start:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

.btn-start.secondary {
  background: #555;
}

.btn-start.secondary:hover {
  background: #333;
}
