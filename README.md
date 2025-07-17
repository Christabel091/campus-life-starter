// RouteHeader.jsx
export default function RouteHeader({ onBack }) {
  return (
    <div className="route-header">
      <i className="fa fa-arrow-left go-back" onClick={onBack}></i>
      <div>
        <p>Find best route to your spot</p>
        <p>Your spot, your way</p>
      </div>
    </div>
  );
}
