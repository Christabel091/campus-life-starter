/**
 * rankPaths.js (updated)
 * 
 * Ranks paths from dynamicPathFinder using real-world driving metrics:
 * - Closest to user (driving duration)
 * - Closest to destination (driving duration)
 * - Cheapest (total weighted cost)
 * - Most reliable (lowest unreliability, if tracked)
 */

export function rankPaths(paths, userLocation, destinationLocation) {
  const ranked = {
    closestToUser: [...paths].sort((a, b) => {
      const aTime = a.goal.drivingMinutesFromUser || a.totalCost;
      const bTime = b.goal.drivingMinutesFromUser || b.totalCost;
      return aTime - bTime;
    }),

    closestToDestination: [...paths].sort((a, b) => {
      const aTime = a.goal.drivingMinutesFromDestination || a.totalCost;
      const bTime = b.goal.drivingMinutesFromDestination || b.totalCost;
      return aTime - bTime;
    }),

    cheapest: [...paths].sort((a, b) => a.totalCost - b.totalCost),

    secondClosestToUser: [],
    secondClosestToDestination: []
  };

  ranked.secondClosestToUser = ranked.closestToUser[1] ? [ranked.closestToUser[1]] : [];
  ranked.secondClosestToDestination = ranked.closestToDestination[1] ? [ranked.closestToDestination[1]] : [];

  return ranked;
}

// NOTE: To enable driving-based ranking:
// In buildGraph(), set:
//   node.drivingMinutesFromUser = drivingData.durationInTrafficMin
//   node.drivingMinutesFromDestination = await getDrivingData(node.lat, node.lng, dest.lat, dest.lng).durationInTrafficMin











import React, { useEffect, useState } from "react";
import { dynamicPathFinder } from "../utils/dynamicPathFinder";
import { rankPaths } from "../utils/rankPaths";
import { buildGraph } from "../utils/buildGraph";
import MapView from "./MapView";
import Select from "@/components/ui/select";

export default function RouteOptionsPanel({ userLocation, nearbySpots, destinationLocation }) {
  const [paths, setPaths] = useState([]);
  const [rankedPaths, setRankedPaths] = useState({});
  const [selectedRankType, setSelectedRankType] = useState("closestToUser");
  const [activePath, setActivePath] = useState(null);

  useEffect(() => {
    const loadPaths = async () => {
      const { userNode, spotNodes } = await buildGraph(userLocation, nearbySpots);
      const allPaths = dynamicPathFinder(userNode, spotNodes);
      setPaths(allPaths);

      const ranked = rankPaths(allPaths, userLocation, destinationLocation);
      setRankedPaths(ranked);
      if (ranked.closestToUser?.[0]) {
        setActivePath(ranked.closestToUser[0]);
      }
    };
    loadPaths();
  }, [userLocation, nearbySpots, destinationLocation]);

  const handleRankTypeChange = (e) => {
    const value = e.target.value;
    setSelectedRankType(value);
    const newPath = rankedPaths[value]?.[0];
    if (newPath) setActivePath(newPath);
  };

  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-4 p-4">
      <div className="md:col-span-2">
        <MapView path={activePath?.path} />
      </div>

      <div className="md:col-span-1 bg-white rounded-2xl p-4 shadow">
        <h2 className="text-xl font-semibold mb-2">Select Route Option</h2>
        <select value={selectedRankType} onChange={handleRankTypeChange} className="w-full p-2 rounded border">
          <option value="closestToUser">Closest to Me</option>
          <option value="secondClosestToUser">2nd Closest to Me</option>
          <option value="closestToDestination">Closest to Destination</option>
          <option value="secondClosestToDestination">2nd Closest to Destination</option>
          <option value="cheapest">Cheapest Option</option>
        </select>

        {activePath && (
          <div className="mt-4">
            <h3 className="font-medium">Selected Route Summary:</h3>
            <p><strong>Goal Spot:</strong> {activePath.goal.id}</p>
            <p><strong>Total Cost:</strong> {activePath.totalCost.toFixed(2)}</p>
            <p><strong>Spots in Path:</strong> {activePath.path.map(p => p.id).join(" → ")}</p>
          </div>
        )}
      </div>
    </div>
  );
}
