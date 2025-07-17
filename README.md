const RouteControls = ({ onToggleTravelMode, onHideSeen, isDriving }) => (
  <div className="route-filter">
    <DestSearch onSelect={(loc) => setDestinationLocation(loc)} />
    <div className="filter-button">
      <button>Rotate Map</button>
      <button onClick={onToggleTravelMode}>
        <i className={`fas fa-${isDriving ? "car" : "walking"}`}></i>
        <p>{isDriving ? "Driving" : "Walking"}</p>
      </button>
      <button onClick={onHideSeen}>Hide seen spots</button>
    </div>
  </div>
);
