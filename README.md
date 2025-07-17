{rankedPaths[selectedRankType]?.slice(0, 5).map((path, index) => (
  <div
    key={index}
    className="ranked-spot-item"
    onClick={() => {
      setActivePath(path);
      getGoogleDirections(userLocation, path.path[path.path.length - 1], isDriving);
      setEndLocation(path.path[path.path.length - 1]);
    }}
  >
    <p>Lot: {path.goal.lotName}</p>
    <p>Distance: {Math.round(path.totalDistance)}m</p>
    <p>ETA: {Math.round(path.goal.drivingMinutesFromUser)} min</p>
    <p>Price: ${path.totalPrice}</p>
  </div>
))}
