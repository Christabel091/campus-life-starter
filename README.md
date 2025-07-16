/**
 * rankPaths.js
 * 
 * Ranks paths from dynamicPathFinder by different strategies:
 * - Closest to user
 * - Closest to destination
 * - Cheapest (lowest total cost)
 * - Most reliable (lowest unreliability score)
 */

import { getDistance } from "./heuristics";

export function rankPaths(paths, userLocation, destinationLocation) {
  const ranked = {
    closestToUser: [...paths].sort((a, b) => {
      const aDist = getDistance(userLocation.lat, userLocation.lng, a.goal.lat, a.goal.lng);
      const bDist = getDistance(userLocation.lat, userLocation.lng, b.goal.lat, b.goal.lng);
      return aDist - bDist;
    }),

    closestToDestination: [...paths].sort((a, b) => {
      const aDist = getDistance(destinationLocation.lat, destinationLocation.lng, a.goal.lat, a.goal.lng);
      const bDist = getDistance(destinationLocation.lat, destinationLocation.lng, b.goal.lat, b.goal.lng);
      return aDist - bDist;
    }),

    cheapest: [...paths].sort((a, b) => a.totalCost - b.totalCost),

    secondClosestToUser: [],
    secondClosestToDestination: [],
  };

  // Optionally add second closest as separate ranks
  ranked.secondClosestToUser = ranked.closestToUser[1] ? [ranked.closestToUser[1]] : [];
  ranked.secondClosestToDestination = ranked.closestToDestination[1] ? [ranked.closestToDestination[1]] : [];

  return ranked;
}

// Usage example:
// const ranked = rankPaths(paths, userLocation, destinationLocation);
// ranked.closestToUser[0] => Best path by proximity to user
// ranked.cheapest[0] => Lowest cost path
