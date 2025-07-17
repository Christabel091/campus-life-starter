// RoutePage.jsx
import React, { useState, useEffect, useRef } from "react";
import { useNavigate } from "react-router-dom";
import { useMap } from "./MapContext";
import DestSearch from "./DestSearch";
import Body from "./Body";
import MakeReservation from "./MakeReservation";
import RouteHeader from "./components/RouteHeader";
import RouteControls from "./components/RouteControls";
import RouteRankSelector from "./components/RouteRankSelector";
import RouteSummary from "./components/RouteSummary";
import RouteStats from "./components/RouteStats";
import { buildGraph, dynamicPathFinder, getDistance } from "../utils/Huristic";
import { rankPaths } from "../utils/RankPath";
import { formatTime } from "../utils/formatTime";

const RoutePage = ({
  spots,
  setSpots,
  setSelectedSpot,
  setShowModal,
  setActive,
  activeFilters,
  locked,
  setLocked,
  lockedSpotId,
  setLockedSpotId,
  setFreeCount,
  destinationLocation,
  setDestinationLocation,
  setIsRoutingToHome,
  searchKeyword,
  setSearchKeyword,
  isReserveBtnClicked,
  setIsReserveBtnClicked,
}) => {
  const navigate = useNavigate();
  const userLocation = useRef({ lat: 35.8457602, lng: -86.3789569 }).current;
  const { map } = useMap();
  const [clicked, setClicked] = useState(false);
  const [mode, setMode] = useState("user-to-spot");
  const [routePath, setRoutePath] = useState([]);
  const [startLocation] = useState(userLocation);
  const [endLocation, setEndLocation] = useState(null);
  const [stats, setStats] = useState({});
  const [isDriving, setIsDriving] = useState(true);
  const [heading, setHeading] = useState(0);
  const [reserved, setReserved] = useState(false);
  const [showMakeReservation, setShowMakeReservation] = useState(false);
  const [noReservationCnt, setNoReservationCnt] = useState(0);
  const [activePath, setActivePath] = useState(null);
  const [rankedPaths, setRankedPaths] = useState([]);
  const [selectedRankType, setSelectedRankType] = useState("");
  const [eta, setEta] = useState(0);
  const [paths, setPaths] = useState([]);
  const [freeSpots, setFreeSpots] = useState([]);

  const fetchSpotsCloseToDestination = async () => {
    let tempSpots = [];
    let radius = 200;
    while (tempSpots.length < 1) {
      const response = await fetch(
        `http://localhost:3000/map/spots?lat=${destinationLocation.lat}&lng=${destinationLocation.lng}&radius=${radius}`
      );
      const data = await response.json();
      if (!data || data.length < 1) {
        radius += 200;
        continue;
      }
      tempSpots = data;
      const nearByFreeSpots = data.filter((spot) => !spot.isOccupied);
      setFreeSpots(nearByFreeSpots);
      return nearByFreeSpots;
    }
  };

  const loadPaths = async (nearByFreeSpots, value) => {
    const { userNode, spotNodes } = await buildGraph(
      userLocation,
      nearByFreeSpots,
      destinationLocation
    );
    const allPaths = dynamicPathFinder(userNode, spotNodes);
    setPaths(allPaths);
    const ranked = rankPaths(allPaths, userLocation, destinationLocation);
    setRankedPaths(ranked);

    const bestPath = ranked[value]?.[0];
    if (bestPath) {
      setActivePath(bestPath);
      setRoutePath(bestPath.path);
      const pathStats = computeStats(bestPath.path);
      setStats(pathStats);
    }
  };

  const handleRankTypeChange = async (e) => {
    const value = e.target.value;
    setSelectedRankType(value);
    let nearByFreeSpots = [];
    if (value.includes("Destination")) {
      nearByFreeSpots = await fetchSpotsCloseToDestination();
    } else {
      nearByFreeSpots = spots.filter((spot) => !spot.isOccupied);
      setFreeSpots(nearByFreeSpots);
    }
    loadPaths(nearByFreeSpots, value);
  };

  useEffect(() => {
    if (eta > 0 && eta <= 600 && noReservationCnt < 3) {
      setShowMakeReservation(true);
    }
  }, [eta]);

  useEffect(() => {
    if (reserved) {
      const spot = spots.find(
        (spot) =>
          spot.coordLat === endLocation.lat &&
          spot.coordLng === endLocation.lng
      );
      if (spot) {
        setSelectedSpot(spot);
        setSearchKeyword(spot.lotName);
        navigate("/Home/ReserveDetails");
        setTimeout(() => {
          setIsReserveBtnClicked(true);
        }, 2000);
        setReserved(false);
      }
    }
  }, [reserved]);

  const computeStats = (path) => {
    if (!path || path.length < 2) return { totalDistance: 0, eta: 0 };
    let distance = 0;
    for (let i = 1; i < path.length; i++) {
      distance += getDistance(
        path[i - 1].lat,
        path[i - 1].lng,
        path[i].lat,
        path[i].lng
      );
    }
    distance = distance * 1000;
    let speed = isDriving ? 10 : 1.4;
    const currentHour = new Date().getHours();
    speed *= getSpeedWithMutiplier(currentHour);
    speed = (speed * 1000) / 3600;
    const etaSeconds = Math.round(distance / speed);
    setEta(etaSeconds);
    return {
      totalDistance: `${Math.round(distance)}m`,
      eta: formatTime(etaSeconds),
      accuracy: "90%",
    };
  };

  const getSpeedWithMutiplier = (hour) => {
    if (hour >= 7 && hour < 11) return 0.6;
    if (hour >= 11 && hour < 14) return 0.8;
    return 1;
  };

  return (
    <>
      <RouteHeader
        setIsRoutingToHome={setIsRoutingToHome}
        navigate={navigate}
      />
      <DestSearch onSelect={(loc) => setDestinationLocation(loc)} />
      <RouteControls isDriving={isDriving} setIsDriving={setIsDriving} />
      <div className="site-container">
        <RouteRankSelector
          selectedRankType={selectedRankType}
          handleRankTypeChange={handleRankTypeChange}
          activePath={activePath}
        />
        <Body
          mode="route"
          routeMode={mode}
          name="Your Smart Router"
          spots={spots}
          setSpots={setSpots}
          setSelectedSpot={setSelectedSpot}
          setShowModal={setShowModal}
          setActive={setActive}
          activeFilters={activeFilters}
          userLocation={userLocation}
          locked={locked}
          setLocked={setLocked}
          lockedSpotId={lockedSpotId}
          setLockedSpotId={setLockedSpotId}
          setFreeCount={setFreeCount}
          routePath={routePath}
          destinationLocation={destinationLocation}
          endLocation={endLocation}
          heading={heading}
          setSearchKeyword={setSearchKeyword}
          setIsReserveBtnClicked={setIsReserveBtnClicked}
        />
      </div>
      <RouteStats stats={stats} />
      {showMakeReservation && (
        <MakeReservation
          setReserve={setReserved}
          setShowMakeReservation={setShowMakeReservation}
          noReservationCnt={noReservationCnt}
          setNoReservationCnt={setNoReservationCnt}
        />
      )}
    </>
  );
};

export default RoutePage;
