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
  padding-top: 20rem;
  text-align: center;

  width: 100%;
  max-width: 500px;
  margin: 10rem auto;
  background: #ffffff;
  border-radius: 10px;
  padding: 2.5rem 2rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease;
}
@keyframes drop {
  0% {
    opacity: 0;
    transform: translateY(-200px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
.welcome-page .spotOn {
  opacity: 0;
  animation: drop 0.3s ease-out forwards;
  animation-delay: 0.5s;
}
.welcome-page h1 {
  font-size: 3rem;
  color: black;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  margin-bottom: 1rem;
}

.welcome-page p {
  font-size: 1.125rem;
  color: #666;
  margin-bottom: 1rem;
}

.welcome-page .btn-start {
  display: inline-block;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(45deg, #1c2e46 0%, #7c94b4 100%);
  border: none;
  border-radius: 8px;
  margin: 1rem;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease, background 0.3s ease;
}

.welcome-page .btn-start:hover {
  transform: translate(-4px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.25);
  background: linear-gradient(45deg, #7c94b4 0%, #1c2e46 100%);
}   /** @format */

import { Link } from "react-router-dom";
import "../styles/pages.css";
const welcome = () => {
  return (
    <div className="overlay">
      <div className="welcome-page">
        <h1 className="spotOn">SpotOn</h1>
        <p>Parking made easier for YOU</p>
        <p>find the parking spot for you</p>
        <Link to="/signup">
          <button className="btn-start">Sign In</button>
        </Link>
        <Link to="/login">
          <button className="btn-start">Login</button>
        </Link>
      </div>
    </div>
  );
};

export default welcome;
