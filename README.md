// TopSpotsList.jsx
import React from 'react';

const TopSpotsList = ({ rankedPaths, onSelect }) => {
  if (!rankedPaths || !rankedPaths.length) return null;

  return (
    <div className="top-spots-list">
      {rankedPaths.slice(0, 5).map((path, idx) => (
        <div key={idx} className="spot-item" onClick={() => onSelect(path)}>
          <h4>{path.goal?.lotName || `Spot ${idx + 1}`}</h4>
          <p>${path.totalPrice}/hr — ETA: {path.goal?.drivingMinutesFromUser} mins</p>
        </div>
      ))}
    </div>
  );
};

export default TopSpotsList;


// RankSelector.jsx
import React from 'react';

const RankSelector = ({ onChange }) => (
  <div className="rank-options">
    <p onClick={() => onChange('closestToUser')}>Closest to You</p>
    <p onClick={() => onChange('closestToDestination')}>Closest to Destination</p>
    <p onClick={() => onChange('cheapest')}>Cheapest</p>
  </div>
);

export default RankSelector;
