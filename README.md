import React, { useEffect, useState } from "react";
import { dynamicPathFinder } from "../utils/dynamicPathFinder";
import { rankPaths } from "../utils/rankPaths";
import { buildGraph } from "../utils/buildGraph";
import MapView from "./MapView";
import {
  Sheet,
  SheetContent,
  SheetTrigger,
  SheetHeader,
  SheetTitle,
} from "@/components/ui/sheet";
import { Select, SelectItem, SelectTrigger, SelectValue, SelectContent } from "@/components/ui/select";
import { Button } from "@/components/ui/button";

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

  const handleRankTypeChange = (value: string) => {
    setSelectedRankType(value);
    const newPath = rankedPaths[value]?.[0];
    if (newPath) setActivePath(newPath);
  };

  return (
    <div className="flex flex-col md:flex-row gap-4 p-4">
      <div className="flex-1">
        <MapView path={activePath?.path} />
      </div>

      <Sheet>
        <SheetTrigger asChild>
          <Button variant="default" className="mt-4 md:mt-0">View Route Options</Button>
        </SheetTrigger>
        <SheetContent side="right" className="w-[300px] sm:w-[400px]">
          <SheetHeader>
            <SheetTitle>Select Route Option</SheetTitle>
          </SheetHeader>

          <div className="mt-4">
            <Select value={selectedRankType} onValueChange={handleRankTypeChange}>
              <SelectTrigger className="w-full">
                <SelectValue placeholder="Select a route" />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="closestToUser">Closest to Me</SelectItem>
                <SelectItem value="secondClosestToUser">2nd Closest to Me</SelectItem>
                <SelectItem value="closestToDestination">Closest to Destination</SelectItem>
                <SelectItem value="secondClosestToDestination">2nd Closest to Destination</SelectItem>
                <SelectItem value="cheapest">Cheapest Option</SelectItem>
              </SelectContent>
            </Select>

            {activePath && (
              <div className="mt-6 text-sm space-y-2">
                <div><strong>Goal Spot:</strong> {activePath.goal.id}</div>
                <div><strong>Total Cost:</strong> {activePath.totalCost.toFixed(2)}</div>
                <div><strong>Spots in Path:</strong> {activePath.path.map(p => p.id).join(" → ")}</div>
              </div>
            )}
          </div>
        </SheetContent>
      </Sheet>
    </div>
  );
}
